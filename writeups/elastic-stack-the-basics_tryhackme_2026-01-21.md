# Elastic Stack: The Basics

---

Security Information and Event Management (SIEM) centralizes log collection from diverse sources, normalizing formats, correlating 
events, and triggering alerts via detection rules to enable efficient threat identification. Host-centric logs capture internal 
activities—file access, authentication attempts, process executions, registry modifications, PowerShell runs—from endpoints like 
Windows or Linux machines. Network-centric logs record inter-host or internet interactions—SSH connections, FTP transfers, web 
traffic, VPN access, file sharing—from devices such as firewalls, intrusion detection/prevention systems, or routers.

Isolated logs pose challenges: scattered across numerous devices generating hundreds of events per second, requiring individual 
connections via SSH or RDP for review; lacking centralization hinders timely analysis; single logs offer limited context, missing 
patterns like lateral movement preceding file access; manual examination of high volumes proves impractical for humans; varying 
formats demand familiarity with each source.

SIEM addresses these through centralized collection via agents or APIs, pulling logs into one repository. Normalization parses raw 
entries into consistent field-value pairs. Correlation uncovers relationships, transforming seemingly benign sequences—unusual VPN 
login, document access, PowerShell execution, outbound connection—into indicators of data exfiltration from compromised credentials. 
Real-time alerting fires when rule conditions match, notifying analysts for investigation within the platform. Dashboards summarize 
insights—alert highlights, notifications, health alerts, failed logins, ingested events count, triggered rules, top domains—supporting 
default and custom views.

Ingestion methods include lightweight agents (forwarders in Splunk) installed on endpoints, Syslog protocol for real-time transfer 
from servers or databases, manual uploads for offline data, port-forwarding to listening instances.

Detection rules define logical conditions: multiple failed logins in seconds for brute force, successful login post-failures, USB 
insertions under restrictions, outbound exceeding thresholds for exfiltration. Examples leverage Windows Event IDs—104 for log 
clearing attempts, 4688 for process creation combined with commands like whoami. Normalized fields enable precise rule targeting.

Alert investigation examines associated events and flows, verifies met conditions, classifies true or false positive. False positives 
prompt rule tuning; true positives trigger deeper probes, asset owner contact, host isolation, or IP blocking.

I find the correlation capability particularly revealing—isolated events rarely tell the full story, but chained views expose campaigns 
that manual per-device checks would miss.

---

### Key Takeaways
- Collect host-centric logs capturing file access, authentication attempts, process executions, registry modifications, PowerShell runs
- Gather network-centric logs recording SSH connections, FTP transfers, web traffic, VPN access, file sharing
- Ingest via agents/forwarders on endpoints, Syslog for real-time, manual uploads for offline, port-forwarding to listeners
- Define detection rules as logical conditions: multiple failed logins in seconds, successful post-failures, USB insertions, outbound exceeding thresholds
- Trigger alerts on event ID 104 for log clearing, 4688 with whoami for suspicious commands
- Investigate by examining events/flows, checking rule conditions, classifying true/false positive
- Tune rules for false positives, escalate true positives for investigation, contact owners, isolate hosts, block IPs

---

