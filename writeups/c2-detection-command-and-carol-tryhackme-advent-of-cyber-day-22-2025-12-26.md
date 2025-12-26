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
## Parts of RITA 

---
The **Details pane** in RITA serves as the granular inspection point for any suspicious network entry. Once a connection is selected 
from the results list, this pane provides the necessary context to determine if a signal is a false positive or a legitimate threat. 
The information is divided into two primary categories: **Threat Modifiers** and **Connection Info**.

### Threat Modifiers
These modifiers are the core analytical components RITA uses to calculate severity. They look beyond simple 
packet counts to find behavioral anomalies that are difficult for attackers to mask.

* **MIME type/URI mismatch:** This flags instances where the declared file type in an HTTP header does not match the actual file
  extension or content type in the URI. Attackers often use this technique to smuggle malicious scripts or executables disguised as
  harmless images or text files.
* **Rare signature:** This identifies unique or uncommon TLS fingerprints, SSL certificate details, or User-Agent strings. Because
  legitimate browsers and applications usually have consistent signatures across an enterprise, a single host using a unique "one-off"
  signature often indicates custom malware or a C2 framework.
* **Prevalence:** This measures the commonality of a destination across the entire network. If a high percentage of internal hosts
  are visiting a site (like a known update server), it is likely benign. Conversely, if only one or two hosts are communicating with
  an external IP, it is considered highly suspicious.
* **First Seen:** This provides the timestamp of the very first time an external host appeared in the network logs. New, previously
  unknown external destinations are prioritized for review, as they often correspond with the stand-up of fresh attacker infrastructure.
* **Missing host header:** Standard web traffic almost always includes a Host header in HTTP requests. Its absence is a common
  indicator of automated scripts or direct socket communication used by malware.
* **Large amount of outgoing data:** This is a primary indicator of data exfiltration. While many connections are download-heavy
  (streaming, updates), a connection that uploads significant volumes of data to an external source warrants immediate investigation.
* **No direct connections:** This identifies complex communication chains where traffic does not follow a direct path, potentially
  indicating the use of proxies or more sophisticated, indirect C2 channels.

### Connection Info
While Threat Modifiers focus on behavior, the **Connection Info** section provides the raw metadata of the traffic. This data helps analysts verify the technical details of the suspected breach.

* **Connection count:** High frequency is a hallmark of "strobe" or "beaconing" behavior. A massive number of connections to a
  single IP over a short period suggests an automated heartbeat rather than human activity.
* **Total bytes sent:** This represents the cumulative volume of data transmitted. High outbound totals, especially when compared
  to inbound data, can confirm exfiltration.
* **Port, Protocol, and Service:** This identifies the destination port and the protocol being used (e.g., TCP/UDP). If a common
  service like HTTP is running on a non-standard port, or if an encrypted port like 443 lacks a proper SSL/TLS handshake, it is
  a high-fidelity indicator of a covert channel.

---
| Description | Code/Command |
| --- | --- |
| View specific beacon data | `rita show-beacons <database>` |
| View long-duration connections | `rita show-long-connections <database>` |
| Search filter for high beacons | `/beacon:>=90` |
| Filter by destination domain | `/dst:malicious-domain.com` |
| Filter by internal source IP | `/src:192.168.1.50` |

---
### Key Takeaways - **Threat Modifiers** prioritize entries based on how "un-human" or anomalous the communication appears relative to the rest of the network.
* **Prevalence** is one of the most effective ways to filter out noise, as common enterprise traffic will have high prevalence scores.
* **Connection Info** provides the empirical evidence (bytes, ports, counts) needed to justify a deeper investigation into a
  specific host.
* A **High Severity** score is rarely the result of a single factor; it is typically an aggregation of multiple modifiers
  (e.g., rare signature + low prevalence + high beacon score).

---


