# Intro to Logs

---

Logs are a fundamental source of historical evidence in cybersecurity. They preserve 
records of system and application activity, allowing analysts to reconstruct events, 
identify suspicious behaviour, and investigate potential security incidents. Authentication attempts, file access, network 
connections, configuration changes, and system errors all generate records that contribute to an organisation's security visibility.

Effective log analysis requires more than collecting records. Individual entries may appear insignificant, but correlating 
information from different sources can reveal an attacker's activities, the sequence of events, the systems affected, and the 
outcome of an intrusion. A useful log entry generally contains a timestamp, the originating system or application, the event type, 
and additional context such as the initiating user or device address.

The practical scenario involved SwiftSpend Financial, whose web server was receiving repeated adversarial scans. The objective was 
to configure logging, collect relevant evidence, and investigate the activity. The exercise used a Linux-based virtual machine with
restricted administrative privileges. Privilege escalation was unnecessary because the available commands were sufficient for the 
tasks.

Log types provide different perspectives on system activity. Application logs record application status, warnings, and errors. Audit
logs document operational activities relevant to compliance. Security logs capture authentication, permission changes, and firewall 
events. Server and system logs record server operations, kernel activity, boot sequences, and hardware conditions. Network logs capture
traffic and connections, while database logs record queries and updates. Web server logs document incoming requests, requested 
resources, and response codes.

Log formats determine how information is organised and represented. Semi-structured formats include Syslog and Windows Event Log 
(EVTX). Structured formats include Comma-Separated Values (CSV), Tab-Separated Values (TSV), JavaScript Object Notation (JSON),
W3C Extended Log Format (ELF), and eXtensible Markup Language (XML). Unstructured formats include the NCSA Common Log Format (CLF) 
and NCSA Combined Log Format. Custom formats can also be developed for specialised applications, although they may require dedicated
parsing tools.

Log standards provide specifications for generating, transmitting, and retaining records. Relevant examples include Common Event 
Expression (CEE), developed by MITRE; the Open Worldwide Application Security Project (OWASP) Logging Cheat Sheet; the Syslog 
Protocol; National Institute of Standards and Technology (NIST) Special Publication 800-92; Azure Monitor Logs; Google Cloud Logging;
Oracle Cloud Infrastructure Logging; and Virginia Tech's Standard for Information Technology Logging.

Reliable log collection depends on identifying relevant sources and maintaining accurate timestamps. Network Time Protocol (NTP) 
synchronisation helps establish a consistent event timeline. Collection begins by identifying potential sources, selecting an 
appropriate collector, configuring event and time parameters, and testing whether records are being received correctly. The exercise 
also demonstrated synchronisation using an external NTP server, although the laboratory machine's lack of internet connectivity 
prevented reproducing that configuration directly.

Log management involves secure storage, systematic organisation, regular backups, and periodic reviews. Centralisation brings records
from different sources into a unified environment, improving accessibility and investigation efficiency. Elastic Stack and Splunk 
were presented as examples of centralised logging platforms. Their integration with monitoring and incident management systems 
supports event detection, notifications, and coordinated response.

The practical collection exercise used rsyslog to capture Secure Shell Daemon (sshd) messages in a dedicated file. The configuration 
specified file permissions and directed messages from the SSH service into the designated log destination. After creating the
configuration, rsyslog was restarted, and a localhost SSH connection was used to generate test activity. The resulting records could 
then be inspected. Where remote forwarding is unavailable, SCP and rsync provide alternative methods for manually transferring 
collected logs.

Storage decisions depend on security requirements, accessibility, capacity, cost, compliance obligations, retention periods, and 
disaster recovery requirements. The exercise divided retention into three categories: Hot Storage for approximately three to six
months, Warm Storage for six months to two years, and Cold Storage for two to five years. Hot records prioritise rapid querying, Warm 
records remain accessible for less immediate investigations, and Cold records are archived for retrospective analysis and scoping.

Log deletion requires a defined policy and appropriate backups, particularly for records that may be needed in future investigations.
Retention and deletion procedures should reflect business and legal requirements, undergo periodic review, and be automated where 
appropriate. Sensitive logs should be encrypted, and backups should be maintained before deletion. These practices also support 
compliance with privacy regulations such as the General Data Protection Regulation (GDPR).

The logrotate exercise demonstrated automated rotation, compression, and removal of log files. A dedicated configuration was created 
for the SSH log destination, with daily rotation and retention of 30 rotated files. A post-rotation script recorded the execution date,
calculated SHA-256 hashes of available compressed log files, saved the results in a separate record, and restarted rsyslog. This
provided a practical demonstration of maintaining hash records alongside rotated logs.

Log analysis follows a sequence of operations beginning with data sources and continuing through parsing, normalisation, sorting, 
classification, enrichment, correlation, visualisation, and reporting. Parsing separates raw records into usable fields. Normalisation 
standardises those fields across different sources, making comparisons easier. Sorting organises records by timestamps, sources, 
event types, or severity. Classification groups records according to their characteristics, while enrichment adds contextual 
information such as user details, geographical information, or threat intelligence.

Correlation connects related events across different records, helping identify relationships that individual entries might conceal. 
Visualisation presents findings through graphical representations, including charts, graphs, and heat maps. Reporting consolidates the
results into understandable summaries for security teams, management, and auditors.

Security Information and Event Management (SIEM) platforms such as Splunk and Elastic Search support more extensive analysis. For 
immediate investigations, Linux utilities including cat, grep, sed, sort, uniq, awk, and sha256sum provide practical alternatives. 
Windows investigations can use EZ-Tools and the Get-FileHash cmdlet. Hashing log files during acquisition helps establish their
integrity and supports their evidential handling.

Several analytical techniques complement the processing workflow. Pattern recognition identifies recurring behaviour, while anomaly
detection highlights deviations from expected activity. Correlation analysis connects related events and assists root cause 
investigations. Timeline analysis examines activity over time. Machine learning and Artificial Intelligence (AI) can support automated 
classification, enrichment, predictive analysis, and responses. Statistical analysis, including regression and hypothesis testing,
provides quantitative insights.

The practical investigation demonstrated two approaches to examining collected records. The first used an open-source Log Viewer to
inspect unparsed raw log files directly. The second combined records from multiple sources into a consolidated file using Unix 
utilities. The consolidated approach involved normalising timestamps, merging records, filtering relevant entries, sorting 
chronologically, and removing duplicate lines.

The investigation illustrated how combining web server access logs, cron records, SSH authentication logs, and GitLab API activity can
help reconstruct an adversary's actions. The scenario demonstrated the value of identifying the originating device, establishing 
the sequence of access, determining whether exposed information was obtained, and understanding subsequent post-exploitation activity.

The room concluded by connecting log analysis with broader Security Operations and Incident Response disciplines. Recommended learning
areas included Endpoint Detection and Response (EDR), Intrusion Detection and Prevention Systems (IDPS), and SIEM. Suggested resources
included Intro to Endpoint Security, Aurora EDR, Wazuh, Snort, Snort Challenge - The Basics, Snort Challenge - Live Attacks, 
Investigating with ELK 101, Splunk: Basics, and Incident Handling with Splunk. The next recommended learning room was Log Operations.

---

### Code and Command Reference

\| Description | Code/Command |\
\|---|---|\
\| Inspect available sudo privileges | `sudo -l` |\
\| Display Syslog records | `cat syslog.txt` |\
\| Read Windows Event Logs | `Get-WinEvent -Path "C:\Windows\System32\winevt\Logs\Application.evtx"` |\
\| Read CSV records | `cat log.csv` |\
\| Read JSON records | `cat log.json` |\
\| Read ELF records | `cat elf.log` |\
\| Read XML records | `cat log.xml` |\
\| Read CLF records | `cat clf.log` |\
\| Read Combined records | `cat combined.log` |\
\| Synchronise Linux system time | `ntpdate pool.ntp.org` |\
\| Display system date and time | `date` |\
\| Check rsyslog service status | `sudo systemctl status rsyslog` |\
\| Create rsyslog configuration using gedit | `gedit /etc/rsyslog.d/98-websrv-02-sshd.conf` |\
\| Create rsyslog configuration using nano | `nano /etc/rsyslog.d/98-websrv-02-sshd.conf` |\
\| Create rsyslog configuration using vi | `vi /etc/rsyslog.d/98-websrv-02-sshd.conf` |\
\| Create rsyslog configuration using vim | `vim /etc/rsyslog.d/98-websrv-02-sshd.conf` |\
\| Configure rsyslog file permissions | `$FileCreateMode 0644` |\
\| Route SSH daemon messages | `:programname, isequal, "sshd" /var/log/websrv-02/rsyslog_sshd.log` |\
\| Restart rsyslog | `sudo systemctl restart rsyslog` |\
\| Generate SSH log activity | `ssh localhost` |\
\| Create logrotate configuration using gedit | `sudo gedit /etc/logrotate.d/98-websrv-02_sshd.conf` |\
\| Create logrotate configuration using nano | `sudo nano /etc/logrotate.d/98-websrv-02_sshd.conf` |\
\| Create logrotate configuration using vi | `sudo vi /etc/logrotate.d/98-websrv-02_sshd.conf` |\
\| Create logrotate configuration using vim | `sudo vim /etc/logrotate.d/98-websrv-02_sshd.conf` |\
\| Configure daily rotation, compression, and retention | `daily` / `rotate 30` / `compress` |\
\| Record rotation date and hash values | `DATE=$(date +"%Y-%m-%d"); echo "$(date)" >> "/var/log/websrv-02/hashes_"$DATE"_rsyslog_sshd.txt"` |\
\| Iterate through rotated files | `for i in $(seq 1 30); do FILE="/var/log/websrv-02/rsyslog_sshd.log.$i.gz"; if [ -f "$FILE" ]; then HASH=$(/usr/bin/sha256sum "$FILE" | awk '{ print $1 }'); echo "rsyslog_sshd.log.$i.gz "$HASH"" >> "/var/log/websrv-02/hashes_"$DATE"_rsyslog_sshd.txt"; fi; done` |\
\| Restart rsyslog after rotation | `systemctl restart rsyslog` |\
\| Execute logrotate manually | `sudo logrotate -f /etc/logrotate.d/98-websrv-02_sshd.conf` |\
\| Process web server access logs | `awk -F'[][]' '{print "[" $2 "]", "--- /var/log/gitlab/nginx/access.log ---", "\"" $0 "\""}' /var/log/gitlab/nginx/access.log \| sed "s/ +0000//g" > /tmp/parsed_consolidated.log` |\
\| Process cron records | `awk '{ original_line = $0; gsub(/ /, "/", $1); printf "[%s/%s/2023:%s] --- /var/log/websrv-02/rsyslog_cron.log --- \"%s\"\n", $2, $1, $3, original_line }' /var/log/websrv-02/rsyslog_cron.log >> /tmp/parsed_consolidated.log` |\
\| Process SSH daemon records | `awk '{ original_line = $0; gsub(/ /, "/", $1); printf "[%s/%s/2023:%s] --- /var/log/websrv-02/rsyslog_sshd.log --- \"%s\"\n", $2, $1, $3, original_line }' /var/log/websrv-02/rsyslog_sshd.log >> /tmp/parsed_consolidated.log` |\
\| Process GitLab API JSON records | `awk -F'"' '{timestamp = $4; converted = strftime("[%d/%b/%Y:%H:%M:%S]", mktime(substr(timestamp, 1, 4) " " substr(timestamp, 6, 2) " " substr(timestamp, 9, 2) " " substr(timestamp, 12, 2) " " substr(timestamp, 15, 2) " " substr(timestamp, 18, 2) " 0 0")); print converted, "--- /var/log/gitlab/gitlab-rails/api_json.log ---", "\""$0"\""}' /var/log/gitlab/gitlab-rails/api_json.log >> /tmp/parsed_consolidated.log` |\
\| Filter specific records | `grep "<TARGET_IP>" /tmp/parsed_consolidated.log > /tmp/filtered_consolidated.log` |\
\| Sort consolidated records | `sort /tmp/parsed_consolidated.log > /tmp/sort_parsed_consolidated.log` |\
\| Remove duplicate records | `uniq /tmp/sort_parsed_consolidated.log > /tmp/uniq_sort_parsed_consolidated.log` |\
\| Calculate SHA-256 hashes | `sha256sum <LOG_FILE>` |\
\| Calculate Windows file hash | `Get-FileHash <FILE>` |\
\| Manually transfer logs | `scp` / `rsync` |\
\| Inspect raw logs through Log Viewer | `http://<TARGET_IP>:8111/log?log=<REDACTED_LOG_PATHS>` |\
\| Inspect consolidated logs through Log Viewer | `http://<TARGET_IP>:8111/log?path=%2Ftmp%2Funiq_sort_parsed_consolidated.log` |

---

### Extracted Tables

SwiftSpend Financial investigation scenario

| Question              | Finding                                                                                                                                                              |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| What happened?        | An adversary accessed the organisation's GitLab instance.                                                                                                            |
| When did it happen?   | Access began at a recorded time on September 8, 2023. The precise timestamp has been redacted.                                                                       |
| Where did it happen?  | The activity originated from a device within the VPN Users network segment. Specific IP addresses and network identifiers have been redacted.                        |
| Who was responsible?  | Network logs associated the activity with a device identified through its User-Agent information. Identifying network details have been redacted.                    |
| Were they successful? | Yes. An API key was publicly exposed, and proxy records indicated that the adversary reached the GitLab service and maintained access through an uploaded web shell. |
| What was the result?  | The adversary achieved remote code execution and conducted post-exploitation activities.                                                                             |

---

### Key Takeaways

- Logs provide historical evidence of authentication, file access, network activity, system errors, and configuration changes.
- Correlate records from different sources to reconstruct incidents and establish what happened, when, where, who was involved, and
  the outcome.
- Recognise the distinctions between application, audit, security, server, system, network, database, and web server logs.
- Understand semi-structured, structured, and unstructured formats, including Syslog, EVTX, CSV, TSV, JSON, ELF, XML, CLF, and
  Combined.
- Maintain accurate timestamps through NTP synchronisation.
- Follow the collection sequence: identify sources, select a collector, configure collection parameters, and test collection.
- Manage collected logs through secure storage, organisation, backups, and periodic reviews.
- Centralise records using platforms such as Elastic Stack or Splunk, integrate sources, configure monitoring, and connect incident
  management systems.
- Use rsyslog to collect SSH daemon messages, configure their destination, restart the service, and verify collection through generated
   activity.
- Apply appropriate storage and retention policies using Hot, Warm, and Cold storage.
- Back up important records before deletion, encrypt sensitive logs, and automate retention procedures where appropriate.
- Use logrotate for rotation, compression, retention, and recording SHA-256 hashes of rotated files.
- Follow the analysis sequence: parsing, normalisation, sorting, classification, enrichment, correlation, visualisation, and reporting.
- Use Linux utilities such as cat, grep, sed, sort, uniq, awk, and sha256sum for immediate log investigations.
- Use EZ-Tools and Get-FileHash for Windows-based log analysis and hashing.
- Apply pattern recognition, anomaly detection, correlation analysis, timeline analysis, machine learning, visualisation, and
   statistical analysis.
- Preserve log integrity by calculating hashes during acquisition.
- Examine raw logs when immediate inspection is required, or consolidate and normalise records when chronological correlation across
  multiple sources is necessary.
- Continue developing practical skills through EDR, IDPS, SIEM, Security Operations, and Incident Response training.

---






