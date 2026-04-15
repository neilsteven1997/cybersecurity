# Module 4: Security Hardening 

---

Security hardening was introduced in module 4 as the practice of strengthening systems, devices, networks, applications, and cloud 
infrastructure to reduce vulnerabilities and shrink the attack surface, defined as every potential entry point a threat actor could 
exploit. The module built directly on prior network operations and vulnerability coverage by shifting emphasis to practical hardening 
across operating systems, networks, and cloud environments. Common procedures include applying software patches, altering device and 
application configurations such as enforcing stronger passwords and current encryption standards, removing unused applications and 
services, disabling unused ports, and tightening access permissions to streamline monitoring and limit exposure.

Operating system hardening requires both recurring and one-time actions. Regular tasks center on installing patch updates as soon as 
vendors release them to close known security gaps and on securely wiping old hardware while deleting unused software to eliminate 
unnecessary risk. One-time tasks involve creating a documented baseline configuration, such as specified firewall rules, that serves as
a reference for new builds and for spotting unauthorized modifications, alongside enforcing strong password policies that mandate 
minimum length, capital letters, numbers, symbols, and multi-factor authentication to block unauthorized entry.

Brute force attacks were examined as a trial-and-error method attackers use to discover credentials, with simple variants relying on 
guessed username-password combinations and dictionary attacks drawing on lists of common passwords or previously breached data. These 
attacks can be manual or automated through specialized tools and often succeed when default or weak credentials remain in place. 
Prevention relies on salting and hashing to transform passwords into irreversible, complex values; multi-factor or two-factor 
authentication that demands additional verification such as fingerprints, facial recognition, or one-time passwords; CAPTCHA and 
reCAPTCHA challenges to distinguish humans from automated scripts; and formal password policies that standardize complexity, rotation 
frequency, reuse restrictions, and login attempt limits. Virtual machines provide isolated environments for safely executing suspicious
code or malware, allowing reversion to clean states and minimizing host risk, while sandboxes enable separate testing of patches, bug 
identification, and attack simulation without affecting production networks.

Network hardening follows similar patterns of regular and one-time tasks. Recurring work includes updating firewall rules, performing 
log analysis with security information and event management tools to collect and prioritize events from across the network, applying 
patches, and maintaining server backups. One-time measures encompass port filtering to block or permit specific traffic, assigning 
network access privileges to authorized segments only, implementing network segmentation to isolate departments or zones and contain 
breaches, and encrypting all communications with the latest standards, using stronger encryption for sensitive data. Defense in depth 
layers these controls starting with a firewall that inspects packet headers and, in next-generation versions, payloads, then adds an 
intrusion detection system that monitors for known attack signatures or anomalies and issues alerts, an intrusion prevention system 
that actively blocks suspect traffic, and a security information and event management platform that aggregates logs from intrusion 
detection systems, intrusion prevention systems, firewalls, and other sources into a single dashboard for real-time analysis. Tools 
such as Google Cloud Chronicle and Splunk exemplify these capabilities, though they supplement rather than replace analyst expertise in
a security operations center.

Cloud hardening introduces unique considerations under the shared responsibility model, where the cloud service provider secures the 
underlying infrastructure, physical data centers, hypervisors, and host operating systems while the customer manages configurations, 
identities, data, and applications. Key practices include identity and access management to control user roles and prevent loose 
permissions, hypervisors of type one (running directly on hardware, such as VMware ESXi) or type two (running on host software, such 
as VirtualBox) that CSPs maintain with patches to block virtual machine escapes, baselining to establish consistent environment 
references including restricted admin portals, password management, file encryption, and threat detection, cryptography to encrypt data
at rest and in transit using secure key management, and cryptographic erasure that destroys encryption keys to render data permanently 
unreadable. Additional cloud-specific risks such as expanded attack surfaces from numerous services, zero-day exploits that CSPs can 
patch at the hypervisor level, visibility limitations on provider servers, and rapid service updates necessitate vigilant configuration
and third-party audits.

The module reinforced these concepts through practical activities, including documenting a website compromise involving a brute force 
attack on default administrative credentials that led to embedded malicious JavaScript, tcpdump log analysis identifying DNS and HTTP 
protocols in the traffic flow, and recommendations for remediation such as stronger passwords or two-factor authentication. A parallel 
network hardening analysis examined vulnerabilities like shared passwords, default database credentials, absent firewall rules, and 
missing multi-factor authentication, proposing layered tools and practices to mitigate them. The portfolio activity applied the NIST 
Cybersecurity Framework to a denial-of-service incident caused by an unconfigured firewall permitting an ICMP flood, mapping the event
across identify, protect, detect, respond, and recover functions to produce an incident report suitable for a professional portfolio. 
A coach dialogue session further connected each hardening layer to specific vulnerabilities, emphasizing updates, least privilege, 
segmentation, monitoring, and isolation techniques. The module concluded with a glossary defining core terms such as baseline 
configuration, patch update, penetration testing, security hardening, and security information and event management, alongside a course
wrap-up highlighting the value of continuous learning for securing evolving network environments.

---

## Extracted Tables

| Devices / Tools | Advantages | Disadvantages |
|-----------------|------------|---------------|
| Firewall | A firewall allows or blocks traffic based on a set of rules. | A firewall is only able to filter packets based on information provided in the header of the packets. |
| Intrusion Detection System (IDS) | An IDS detects and alerts admins about possible intrusions, attacks, and other malicious traffic. | An IDS can only scan for known attacks or obvious anomalies; new and sophisticated attacks might not be caught. It doesn’t actually stop the incoming traffic. |
| Intrusion Prevention System (IPS) | An IPS monitors system activity for intrusions and anomalies and takes action to stop them. | An IPS is an inline appliance. If it fails, the connection between the private network and the internet breaks. It might detect false positives and block legitimate traffic. |
| Security Information and Event Management (SIEM) | A SIEM tool collects and analyzes log data from multiple network machines. It aggregates security events for monitoring in a central dashboard. | A SIEM tool only reports on possible security issues. It does not take any actions to stop or prevent suspicious events. |

---

### Key Takeaways
- Security hardening reduces the attack surface through software updates, configuration changes including stronger passwords and
  updated encryption standards, removal of unused applications and services, disabling unused ports, and reduction of access permissions.
- Regular OS hardening tasks consist of installing patch updates immediately upon vendor release and properly disposing of old hardware
  while deleting unused software.
- One-time OS hardening tasks include establishing a documented baseline configuration such as firewall rules for reference and change
  detection plus implementing strong password policies with complexity requirements and multi-factor authentication.
- Brute force prevention measures encompass salting and hashing passwords, multi-factor or two-factor authentication requiring additional
   verification factors, CAPTCHA and reCAPTCHA to block automated scripts, and organizational password policies covering complexity,
  rotation, reuse restrictions, and login attempt limits.
- Regular network hardening tasks include maintaining firewall rules, conducting log analysis with security information and event
  management tools, applying patches, and performing server backups.
- One-time network hardening tasks cover port filtering on firewalls, setting network access privileges, implementing network
  segmentation to isolate zones, and applying the latest encryption standards with stronger protection for sensitive data.
- Cloud security hardening techniques include identity and access management for user roles, hypervisor management and patching,
  baselining for consistent configurations, cryptography for data protection, and cryptographic erasure via key destruction.
- The shared responsibility model assigns infrastructure security to the cloud service provider while placing configuration, identity,
  data, and application responsibilities on the customer organization.
- Defense in depth layers firewalls for traffic control, intrusion detection systems for alerts on signatures and anomalies, intrusion
  prevention systems for active blocking, and security information and event management platforms for centralized log aggregation and
  analysis.

---




  
