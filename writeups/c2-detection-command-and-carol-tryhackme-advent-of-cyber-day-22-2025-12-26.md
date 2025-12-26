# C2 Detection - Command & Carol

---
Real Intelligence Threat Analytics (RITA) is an open-source framework developed by Active Countermeasures for identifying command 
and control (C2) communication through behavioral network analysis. Unlike traditional signature-based intrusion detection systems, 
RITA identifies threats by examining network metadata for patterns consistent with attacker persistence. This approach allows for the 
detection of beaconing, DNS tunneling, and data exfiltration even within encrypted channels. The framework primarily relies on 
statistical correlation of connection durations, timestamps, and IP/port pairs to distinguish legitimate traffic from malicious 
automated heartbeats.

The analysis pipeline begins with Zeek, an open-source network security monitoring tool that converts raw network traffic or packet 
captures into structured logs. Zeek acts as a passive observer, distilling transaction data and extracted content into specific 
files like `conn.log`, `dns.log`, and `ssl.log`. RITA ingests these logs, cross-references internal hosts against external threat 
intelligence feeds, and applies analytical modules to score the likelihood of compromise. Key indicators such as periodic connection 
intervals, excessive or randomized DNS subdomains, and anomalous TLS handshake signatures are correlated to produce a severity score.

Analytic depth is provided through threat modifiers that evaluate environmental prevalence and historical context. Connections are 
flagged for further investigation if they exhibit rare signatures, such as unique User-Agent strings, or if they represent the 
first time an external host has been observed on the network. Analysts use the RITA terminal interface to pivot from high-level 
severity rankings into granular connection metadata, including byte counts and protocol specifics. This methodology is particularly 
effective against low-and-slow C2 channels that bypass traditional perimeter defenses by mimicking standard stateless application 
protocols.

---
| Description | Code/Command |
| --- | --- |
| Convert PCAP to Zeek log directory | `zeek readpcap <pcap_file> <output_directory>` |
| Import Zeek logs into RITA database | `rita import --logs <log_path> --database <db_name>` |
| Open interactive results viewer | `rita view <database_name>` |
| View strobe (high-frequency) connections | `rita show-strobes <database_name>` |
| List C2 beacon candidates | `rita show-beacons <database_name>` |
| Search syntax within interactive view | `/` (followed by term like `dst:ip`) |

---
### Key Takeaways - RITA identifies C2 activity by analyzing the "behavioral fingerprints" of network traffic rather than relying on static signatures.
* High-fidelity results are achieved by correlating connection frequency, duration, and metadata across Zeek's structured log outputs.
* Severity scoring is influenced by threat modifiers like rare certificates, low internal prevalence, and unusual MIME/URI mismatches.
* DNS tunneling is detected by flagging unusually long FQDNs or an excessive volume of unique subdomains for a single root domain.
* Small datasets are susceptible to false positives; a 24-hour traffic window is generally recommended for accurate beaconing analysis.
* The pcaps directory contains example PCAPs of real-life incidents collected from Bradly Duncan's [blog](https://malware-traffic-analysis.net/). He has a wonderful
  collection of malware-related PCAPs that cover real-world threats.

You can learn more about the practical application of these tools in this [guide to finding network anomalies](https://www.youtube.com/watch?v=mpCBOQSjbOA). 
This video provides a detailed walkthrough of using RITA to detect hidden threats on a network, which complements the concepts of behavioral analysis discussed here.

---
