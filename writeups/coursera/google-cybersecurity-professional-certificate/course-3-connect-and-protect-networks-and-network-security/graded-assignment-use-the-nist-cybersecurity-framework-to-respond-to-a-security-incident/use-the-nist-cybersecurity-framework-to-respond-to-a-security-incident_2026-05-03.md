# Graded Assignment: Use the NIST cybersecurity framework to respond to a security incident
**Google Cybersecurity Professional Certificate – Course Activity**  
Neil Steven | May 03, 2026 | Portfolio: [https://github.com/neilsteven1997/cybersecurity](https://github.com/neilsteven1997/cybersecurity/tree/main/writeups/coursera/google-cybersecurity-professional-certificate/course-3-connect-and-protect-networks-and-network-security/graded-assignment-use-the-nist-cybersecurity-framework-to-respond-to-a-security-incident)

## Executive Summary
>[!Note]
>_This scenario is based on a fictional company:_

Scenario

You are a cybersecurity analyst working for a multimedia company that offers web design services, graphic design, and social media marketing solutions to small businesses. Your organization recently experienced a DoS attack, which compromised the internal network for two hours until it was resolved.

During the attack, your organization’s network services suddenly stopped responding due to an incoming flood of ICMP packets. Normal internal network traffic could not access any network resources. The incident management team responded by blocking incoming ICMP packets, stopping all non-critical network services offline, and restoring critical network services. 

The company’s cybersecurity team then investigated the security event. They found that a malicious actor had sent a flood of ICMP pings into the company’s network through an unconfigured firewall. This vulnerability allowed the malicious attacker to overwhelm the company’s network through a denial of service (DoS) attack. 

To address this security event, the network security team implemented: 

- A new firewall rule to limit the rate of incoming ICMP packets

- Source IP address verification on the firewall to check for spoofed IP addresses on incoming ICMP packets

- Network monitoring software to detect abnormal traffic patterns

- An IDS/IPS system to filter out some ICMP traffic based on suspicious characteristics

As a cybersecurity analyst, you are tasked with using this security event to create a plan to improve your company’s network security, following the National Institute of Standards and Technology (NIST) Cybersecurity Framework (CSF). You will use the CSF to help you navigate through the different steps of analyzing this cybersecurity event and integrate your analysis into a general security strategy. We have broken the analysis into different parts in the template below. You can explore them here:

- **Identify** security risks through regular audits of internal networks, systems, devices, and access privileges to identify potential gaps in security. 

- **Protect** internal assets through the implementation of policies, procedures, training and tools that help mitigate cybersecurity threats. 

- **Detect** potential security incidents and improve monitoring capabilities to increase the speed and efficiency of detections. 

- **Respond** to contain, neutralize, and analyze security incidents; implement improvements to the security process. 

**Recover** affected systems to normal operation and restore systems data and/or assets that have been affected by an incident. 



---

**Goal:**_Create an incident report using the knowledge gained about networks throughout this course to analyze a network incident. Analyze the situation using the National Institute of Standards and Technology's Cybersecurity Framework (NIST CSF)._

---

## Incident report analysis: Use the NIST cybersecurity framework to respond to a security incident

### 1. Summary
The organization recently experienced a DoS attack, which compromised the internal network for two hours until it was resolved.  During the attack, the organization’s network services suddenly stopped responding due to an incoming flood of ICMP packets. Normal internal network traffic could not access any network resources. The incident management team responded by blocking incoming ICMP packets, stopping all non-critical network services offline, and restoring critical network services.   

### 2. Identify
The cybersecurity team investigated the security event. They found that a malicious actor had sent a flood of ICMP pings into the company’s network through an unconfigured firewall. This vulnerability allowed the malicious attacker to overwhelm the company’s network through a denial of service (DoS) attack. 

### 3. Protect
To address this security event, the network security team implemented:       
- A new firewall rule to limit the rate of incoming ICMP packets      
- Source IP address verification on the firewall to check for spoofed IP addresses on incoming ICMP packets      
- Network monitoring software to detect abnormal traffic patterns      
- An IDS/IPS system to filter out some ICMP traffic based on suspicious characteristics  

### 4. Detect
1. Update the Incident Response Playbook to include a dedicated ICMP/DoS response section. Define clear step-by-step actions for detection, containment, eradication, and recovery. Use the SIEM as the single pane of glass for real-time monitoring, alerting, log correlation, and incident investigation.
2. Implement regular security awareness training focused on phishing and social engineering. Emphasize that these are common entry points used before launching DoS attacks. Include practical simulations, recognition techniques, and reporting procedures to reduce human-related risks.
3. Conduct quarterly penetration tests specifically targeting all public-facing web assets (websites, portals, marketing platforms). Tests should simulate realistic attacks including DoS, DDoS, and common web vulnerabilities to validate defenses and identify weaknesses proactively.

### 5. Respond
The incident management team responded by blocking incoming ICMP packets, stopping all non-critical network services offline, and restoring critical network services. 

### 6. Recover
- Critical network services were prioritized and restored first after blocking the malicious ICMP traffic.
- Non-critical services were safely taken offline during the attack and brought back online only after the threat was neutralized and network stability was confirmed.
- Firewall rules were updated in real-time to stop the attack and allow normal business operations to resume.
- Verify system functionality and performance after applying new firewall rules (ICMP rate limiting and anti-spoofing).
- Use the newly implemented network monitoring software and IDS/IPS to monitor for any residual or follow-up attacks.
- Conduct a post-incident review to confirm all systems are fully operational and no backdoors or secondary compromises exist.
- Update the Incident Response Playbook with the lessons learned from this DoS event to improve future recovery speed and efficiency.

Reflections/Notes:
The NIST CSF and its five core functions provide a framework of planning proactive to applying reactive measures to cybersecurity threats. These functions are essential for ensuring that an organization has effective security strategies in place. An organization must have the ability to quickly recover from any damage caused by an incident to minimize their level of risk. 

---

## Self-Assessment Score
Completed the Activity 6/6
Self-assessment: Passed (100% / 6 points)

## Artifacts
- Coursera activity: [https://www.coursera.org](https://www.coursera.org/learn/networks-and-network-security/assignment-submission/AFji2/portfolio-activity-use-the-nist-cybersecurity-framework-to-respond-to-a-security/attempt)
- GitHub writeup: [https://github.com/neilsteven1997/cybersecurity](https://github.com/neilsteven1997/cybersecurity/blob/main/writeups/coursera/google-cybersecurity-professional-certificate/course-3-connect-and-protect-networks-and-network-security/graded-assignment-use-the-nist-cybersecurity-framework-to-respond-to-a-security-incident/use-the-nist-cybersecurity-framework-to-respond-to-a-security-incident_2026-05-03.md)

---
Last updated: May 10, 2026









