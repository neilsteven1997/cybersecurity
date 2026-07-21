# Cyber Kill Chain

---

The Cyber Kill Chain framework, introduced by Lockheed Martin in 2011 and inspired by military kill chains, structures cyber attacks 
into seven distinct stages to enable organizations to better understand and disrupt them. It begins with reconnaissance, where attackers
collect details on a target's vulnerabilities and potential entry points through methods that can be either passive, relying on 
open-source intelligence without direct interaction, or active, involving noisier engagements such as social engineering against 
personnel or vulnerability scanning. 

In the weaponisation stage, attackers craft or modify a payload tailored to the target's weaknesses, often employing obfuscation, 
encryption, or embedding it within seemingly benign files like Microsoft Office documents or PDFs, sometimes leveraging exploit kits or
frameworks to prepare a deliverable malicious artifact. Delivery follows, transmitting the prepared payload via channels such as 
phishing emails with attachments or links, spear-phishing tailored to appear legitimate, malicious web links with domain spoofing, 
file-sharing services, malvertising, smishing via SMS, social engineering, or even physical media like USB drives left in accessible 
spots or mailed DVDs with contextual lures.

Once delivered, exploitation occurs by triggering vulnerabilities including weak or default passwords, software flaws in client 
systems or network services, zero-day exploits, injection attacks, or buffer overflows. Successful exploitation leads to installation,
establishing persistence through techniques like scheduled tasks, cron jobs, modified startup scripts, new services or daemons, 
malware deployment, backdoors, rootkits, or living-off-the-land binaries, with web shells sometimes added for command execution over 
HTTPS. 

Command and control then sets up a covert, resilient communication channel back to the attacker's infrastructure, utilizing common 
protocols like HTTP, HTTPS, DNS, or email for blending with normal traffic, tunneling, domain generation algorithms that produce 
thousands of potential domains with only a fraction registered, fast flux for rapidly rotating IP addresses via proxies including 
compromised bots, or legitimate services such as social media or cloud platforms for commands and exfiltration. Finally, actions on 
objectives allow the attacker to achieve goals ranging from data exfiltration and lateral movement to destructive operations, 
ransomware, financial theft, espionage, or manipulation of industrial control systems, often with long-term persistence.

Defensive measures target each stage to break the chain early. For reconnaissance, this includes minimizing public data exposure on 
websites and social media, using WHOIS privacy services, and actively monitoring network traffic and logs for scanning activity. 
Weaponisation defenses emphasize user training on inspecting suspicious emails and attachments, disabling unnecessary features like 
macros via group policy, and reducing the attack surface by uninstalling unneeded software. Delivery countermeasures rely on awareness
training, email and web filtering, web application firewalls, network monitoring, and patch management.

Exploitation is disrupted through strong password policies with multi-factor authentication, timely patching, vulnerability scanning,
intrusion prevention systems, and web application firewalls blocking common attacks like cross-site scripting or request forgery. 
Installation monitoring involves endpoint detection and response tools watching for anomalous processes, file changes, and connections,
alongside system audits, baselines, configuration management, and application allowlisting. Command and control detection uses network
monitoring, firewalls, intrusion detection and prevention systems, DNS query analysis, web traffic inspection, encryption decryption
where feasible, and honeypots. For actions on objectives, data loss prevention, backups, network segmentation, least privilege access
controls, user activity monitoring, and endpoint detection and response prove essential.

The room emphasizes completing the Cyber Security 101 path as a prerequisite. Offensive teams like penetration testers or red teamers
apply this model to simulate realistic adversary behavior and evaluate defenses, while defensive teams such as blue teams focus on 
early interruption to limit damage that typically peaks in the final stage.

---

### Key Takeaways
- The seven stages of the Cyber Kill Chain: Reconnaissance, Weaponisation, Delivery, Exploitation, Installation, Command and Control,
  Actions on Objectives.
- Passive reconnaissance examples include WHOIS queries, DNS lookups, website crawling and scraping, social media analysis, and Google
  Dorking.
- Active reconnaissance examples include network port scanning, vulnerability scanning, and physical site visits.
- Weaponisation often involves exploit kits, malicious macros in Office documents, and obfuscated payloads in common file types.
- Delivery vectors encompass phishing, spear-phishing, malicious links, file-sharing platforms, malvertising, smishing, social
  engineering, and physical media.
- Exploitation targets weak passwords, software vulnerabilities including zero-days, and misconfigurations.
- Installation methods for persistence: scheduled tasks, cron jobs, startup modifications, new services, malware, backdoors, rootkits,
  LOLBins, and web shells.
- Command and control techniques: common protocols, tunneling, DGAs, fast flux, and legitimate service abuse.
- Actions on objectives include data exfiltration, lateral movement, destruction, ransomware, financial fraud, espionage, and ICS
  manipulation.
- Core defenses per stage center on information minimization, user training, patching, monitoring tools like EDR and IPS, segmentation,
   allowlisting, and backups.

---




