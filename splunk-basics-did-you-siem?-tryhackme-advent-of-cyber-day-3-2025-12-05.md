## Log Analysis for Incident Response: Splunk Workflow

This outlines the essential steps I take in Splunk to investigate a web server breach, focusing on command execution 
and log analysis to trace the attacker's path. I'm keeping the queries exactly as they are.

## Initial Data Scan and Triage
First, I need to get eyes on the data. I start by focusing on the web traffic logs, which are our primary source for 
identifying external threats.

-Loading Logs: I run index=main sourcetype=web_traffic and set the time range to "All time" to pull everything in. I 
already know I have two main data sources: web_traffic (connections) and firewall_logs (network access).
-Log Structure Check: I confirm fields like user_agent, path, and client_ip are properly extracted. This verifies the 
logs are usable.
-Timeline Visualization: To spot the attack window, I chart event volume. The query index=main sourcetype=web_traffic | 
timechart span=1d count groups logs by day, making any sudden spike (the attack) obvious. I can then sort this by volume 
using | timechart span=1d count | sort by count | reverse.

## Identifying the Attacker (Anomaly Detection)
I pivot to looking for non-human activity by examining fields for unusual values.

-User Agent Filter: Standard browsers need to be removed to see automated tools. I use index=main 
sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* user_agent!=*Firefox* to 
filter out legitimate traffic.
-IP Aggregation: To confirm the most active suspicious IP, I aggregate the data: sourcetype=web_traffic 
user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* user_agent!=*Firefox* | stats count by client_ip | 
sort -count | head 5. The sort -count places the highest traffic volume at the top, giving me the attacker's IP (<REDACTED>).

## Tracing the Attack Chain
I use the attacker's IP to follow their actions chronologically through the web_traffic logs.

1. Reconnaissance (Footprinting): Probing for exposed configuration files.
sourcetype=web_traffic client_ip="<REDACTED>" AND path IN ("/.env", "/*phpinfo*", "/.git*") | table _time,
path, user_agent, status
2. Enumeration (Vulnerability Testing): Searching for path traversal and redirects.
sourcetype=web_traffic client_ip="<REDACTED>" AND path="*..\/..\/*" OR path="*redirect*" | stats count by path
3. Exploitation (SQL Injection): Looking for automated tool signatures.
sourcetype=web_traffic client_ip="<REDACTED>" AND user_agent IN ("*sqlmap*", "*Havij*") | table _time, path, status
4. Action on Objective (RCE/Ransomware): Confirmation of a web shell and code execution.
-sourcetype=web_traffic client_ip="<REDACTED>" AND path IN ("*bunnylock.bin*", "*shell.php?cmd=*") | table _time,
path, user_agent, status
-Finding: The presence of shell.php?cmd= and execution of a binary (bunnylock.bin) confirms Remote Code Execution (RCE).

## Post-Exploitation and C2 Confirmation
The final step is pivoting to the firewall logs to prove the compromised server communicated externally.

1. Correlate Outbound C2 Communication: I check the firewall logs using the compromised web server's internal IP (10.10.1.5)
   as the source and the attacker's IP as the destination.

-sourcetype=firewall_logs src_ip="10.10.1.5" AND dest_ip="<REDACTED>" AND action="ALLOWED" | table _time, action, protocol, 
src_ip, dest_ip, dest_port, reason
-Finding: The ACTION="ALLOWED" and specific C2 reason/port confirm the successful Command and Control (C2) channel.

2. Volume Exfiltrated: I calculate the total data volume sent out to the attacker's server.
sourcetype=firewall_logs src_ip="10.10.1.5" AND dest_ip="<REDACTED>" AND action="ALLOWED" | stats sum(bytes_transferred)
by src_ip

## Conclusion 
The incident involved a clear progression: Reconnaissance via cURL/Wget probing for configuration files, 
followed by Exploitation using SQLmap payloads. The attacker achieved Remote Code Execution (RCE) via a webshell 
and executed a ransomware-like payload. The firewall logs conclusively confirmed the compromised internal server 
established a successful outbound C2 connection to the external IP.

<details>
<Summary>## Splunk Layout
1. Search query: This query retrieves all events from the main index that were tagged with the custom source type web_traffic.
2. This marks the beginning of the investigation.
3. Time range: The time range is currently set to "All time". In security analysis, this range would be tightened (e.g., to the
4. spike window) after initial data loading.<Summary>
### 5. Timeline: This visual histogram shows the distribution of the 17,172 events over time. The graph indicates the successful
6. daily log volume followed by a distinctive traffic spike (a period of high activity, likely the attack window).
7. Selected fields: These are the fields currently chosen to be displayed in the summary column of the event list (host, source,
8. sourcetype). They represent basic metadata about the log file itself.
### 9. Interesting fields: This pane lists all fields that Splunk has automatically extracted or manually added. Fields prefixed
10. with # (e.g., #date_hour) are automatically generated by Splunk's time commands. The presence of user_agent, path, and
11. client_ip confirms the successful parsing of the web log structure.
12. Event details & field extraction: This section shows the parsed details of a single event with extracted fields like
13. user_agent, path, status, client_ip, and more.

</details>
## You did it! Wareville is one step safer.
The townsfolk are counting on you to keep Christmas secure.
Head back to Wareville to continue your mission!







