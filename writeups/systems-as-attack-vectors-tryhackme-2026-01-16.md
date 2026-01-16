# Systems as Attack Vectors

---
Systems serve as high-value targets in intrusions, offering attackers scale and persistence far beyond single-user compromises. 
A breached endpoint, server, virtual machine, or cloud workload like Microsoft 365 grants control over multiple accounts, 
mailboxes, databases, or entire network segments. The payoff varies by asset: an administrator's laptop unlocks internal 
infrastructure, a mail server exposes thousands of accounts for data theft or blackmail, an industrial control server enables 
ransomware propagation or physical disruption, and a public-facing management interface invites defacement or persistent backdoors.

Access often originates from user actions—malicious USB insertion, pirated software downloads, or credential reuse—with 
stolen or weak passwords implicated in a large majority of breaches. Direct exploitation targets software vulnerabilities; 
tens of thousands of CVEs emerge annually, hundreds actively weaponized in the wild, while zero-days allow pre-disclosure 
compromise until patches arrive. Supply chain attacks compromise trusted updates or libraries, affecting downstream users at 
scale, as demonstrated in major incidents and even TryHackMe's own experience with a compromised animation library.

Misconfigurations introduce equally severe risks when convenience overrides security—default credentials left unchanged, 
permissive cloud storage, or exposed IoT devices recruited into botnets. Unlike bugs, these require no exploit development; 
attackers simply discover and abuse poor setup decisions.

Detection relies on behavioral anomalies—unexpected processes, lateral movement, or privilege escalation—since prevention 
cannot cover every vector. Response differs by root cause: vulnerabilities demand vendor patches with interim mitigations 
such as IP whitelisting, vendor-provided workarounds, or signature-based blocking on IPS/WAF during zero-day windows; 
misconfigurations call for reconfiguration, audits against hardened baselines, periodic vulnerability scanning, and 
penetration testing to surface issues proactively.

Mitigation emphasizes disciplined processes over reactive firefighting. Patch management tracks disclosures and applies 
fixes promptly. Network segmentation and access controls restrict reach. Endpoint protection detects and blocks execution 
attempts. IT training on secure configuration reduces preventable errors.

The SOC role here involves bridging analysis and engineering—identifying patterns in alerts, correlating system anomalies 
with campaign indicators, and advocating controls that reduce noise and risk. Understanding these vectors sharpens 
perspective and strengthens team-wide defenses.

---

Key Takeaways
- Prioritize patch management to close disclosed vulnerabilities quickly and apply interim controls during zero-day exposure
- Enforce network segmentation and least-privilege access to limit lateral movement from compromised systems
- Deploy endpoint detection and response for behavioral monitoring and malware blocking
- Conduct regular configuration audits, vulnerability scanning, and penetration testing to identify misconfigurations
- Train IT staff on secure baselines and risks of convenience-driven setup decisions
- Correlate system-level anomalies with user and network indicators for comprehensive visibility
- Monitor recommended resources for emerging threats: The DFIR Report at https://www.thedfirreport.com, CISA Known Exploited Vulnerabilities Catalog at https://www.cisa.gov/known-exploited-vulnerabilities-catalog, BleepingComputer at https://www.bleepingcomputer.com, Check Point Threat Map at https://threatmap.checkpoint.com

---
