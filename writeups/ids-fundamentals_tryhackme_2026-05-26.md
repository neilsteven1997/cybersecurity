# IDS Fundamentals

---

Firewalls typically sit at network perimeters and inspect traffic during connection establishment, blocking anything that violates 
configured rules. However, once traffic passes through successfully, additional monitoring is required to identify malicious activity 
that occurs afterward. An Intrusion Detection System (IDS) fulfills this role by examining network traffic inside the perimeter for 
signs of compromise or suspicious behavior. In the building security analogy, the firewall functions as the gatekeeper while the IDS 
operates like internal surveillance cameras that monitor ongoing activity and raise alerts on anomalies.

IDS deployments fall into two primary categories based on placement. A Host Intrusion Detection System (HIDS) installs directly on 
individual endpoints and focuses on activities specific to that host, offering deep visibility into local processes and files though 
it becomes operationally burdensome at scale due to resource demands and per-host management. A Network Intrusion Detection System 
(NIDS) monitors traffic across the entire network from a centralized position, providing broader visibility without requiring agents 
on every system.

Detection approaches also vary. Signature-based detection maintains a database of known attack patterns and matches live traffic 
against these signatures, enabling rapid identification of previously documented threats while remaining ineffective against zero-day 
exploits lacking established patterns. Anomaly-based detection establishes a baseline of normal network or system behavior and flags 
deviations, which allows it to catch novel attacks but often produces higher false positive rates when legitimate activity resembles 
malicious patterns. Hybrid systems combine both methods, applying signature matching for known threats and anomaly analysis for unknown
ones to balance strengths and coverage.

Snort, developed in 1998, ranks among the most widely deployed open-source IDS solutions. It primarily employs signature-based 
detection through rule sets stored in dedicated files, with several pre-installed rule collections covering common attack patterns. 
Administrators can enable or disable these built-in rules and supplement them with custom rules tailored to specific environments. The 
tool supports three operational modes: packet sniffer mode for displaying raw traffic without analysis, packet logging mode for 
capturing traffic and alerts in PCAP format for later forensic review, and full NIDS mode for real-time monitoring and alerting based 
on rule matches. The NIDS mode represents Snort's core IDS functionality.

Configuration occurs mainly through the snort.conf file located in /etc/snort, which defines monitored networks via variables such as 
HOME_NET, enabled rule files, and other parameters. Rule files reside in the rules subdirectory. A standard rule follows the structure: 
action protocol source_ip source_port -> destination_ip destination_port (rule options). For example, a basic rule might alert on ICMP
traffic to the home network with a descriptive message, unique signature ID (sid), and revision number for tracking changes.

Custom rules can be added to files such as local.rules. Testing involves running Snort against live interfaces or existing PCAP files
while specifying the configuration and output options. Loopback testing with ping 127.0.0.1 validates rule triggers by generating
console alerts containing timestamp, signature details, priority, protocol, and involved IPs. For offline analysis, Snort processes 
captured PCAP files similarly, supporting forensic investigations of historical traffic.

---

| Description | Code/Command |
|-------------|--------------|
| List Snort directory contents | ls /etc/snort |
| Edit custom rules file | sudo nano /etc/snort/rules/local.rules |
| Sample custom rule for loopback ICMP | alert icmp any any -> 127.0.0.1 any (msg:"Loopback Ping Detected"; sid:10003; rev:1;) |
| Run Snort in NIDS mode with console alerts | sudo snort -q -l /var/log/snort -i lo -A console -c /etc/snort/snort.conf |
| Test rule with loopback ping | ping 127.0.0.1 |
| Run Snort against a PCAP file | sudo snort -q -l /var/log/snort -r Task.pcap -A console -c /etc/snort/snort.conf |

---
  
## Key Takeaways
- Complete prerequisite rooms covering networking fundamentals and core concepts before proceeding with IDS material.
- Signature-based detection excels at known threats but misses zero-days; anomaly-based catches unknowns at the cost of potential false
  positives; hybrid approaches leverage both.
- Enable promiscuous mode on the network interface when using Snort to monitor entire network segments beyond local host traffic.
- Define HOME_NET variable in snort.conf to match the monitored address range.
- Always increment rule revision numbers when modifying existing rules for change tracking.
- Use -r option with PCAP files for offline forensic analysis of captured attack traffic.
- Review generated alerts carefully, noting signature ID, revision, protocol, and IP details for incident correlation.

---









