# Activity: Analyze network attacks
**Google Cybersecurity Professional Certificate – Course Activity**  
Neil Steven | April 02, 2026 | Portfolio: [https://github.com/neilsteven1997/cybersecurity](https://github.com/neilsteven1997/cybersecurity/tree/main/writeups/coursera/google-cybersecurity-professional-certificate/course-3-connect-and-protect-networks-and-network-security/module-3-activity-analyze-network-attacks)

## Executive Summary
>[!Note]
>_This scenario is based on a fictional company:_

Scenario

You work as a security analyst for a travel agency that advertises sales and promotions on the company’s website. The employees of the company regularly access the company’s sales webpage to search for vacation packages their customers might like. 

One afternoon, you receive an automated alert from your monitoring system indicating a problem with the web server. You attempt to visit the company’s website, but you receive a connection timeout error message in your browser.

You use a packet sniffer to capture data packets in transit to and from the web server. You notice a large number of TCP SYN requests coming from an unfamiliar IP address. The web server appears to be overwhelmed by the volume of incoming traffic and is losing its ability to respond to the abnormally large number of SYN requests. You suspect the server is under attack by a malicious actor. 

You take the server offline temporarily so that the machine can recover and return to a normal operating status. You also configure the company’s firewall to block the IP address that was sending the abnormal number of SYN requests. You know that your IP blocking solution won’t last long, as an attacker can spoof other IP addresses to get around this block. You need to alert your manager about this problem quickly and discuss the next steps to stop this attacker and prevent this problem from happening again. You will need to be prepared to tell your boss about the type of attack you discovered and how it was affecting the web server and employees.

**Goal:** _Analyze HTTP Logs captured by a network protocol analyzer to investigate potential malicious activity. Provide the name of the network intrusion attack, and a description of how the attack negatively impacts network performance._



---

## Cybersecurity Incident Report: Network Traffic Analysis

### Part 1: Provide a summary of the problem found in the DNS and ICMP traffic log.
The network protocol analyzer logs indicate that ICMP packets containing the error message: “udp port 53 unreachable.”  when attempting to access the company website www.yummyrecipesforme.com. UDP Port 53 handles most DNS requests on the server. This may indicate a problem with the DNS server or the firewall configuration. It is possible that this is an indication of a malicious attack on the web server. 

## Part 2: Explain your analysis of the data and provide at least one cause of the incident.
The Time incident occurred was 1:24 p.m., 32.192571 seconds. Several customers of clients reported that they were not able to access the client company website www.yummyrecipesforme.com, and saw the error “destination port unreachable” after waiting for the page to load. We attempted to visit the website, and we also received the error “destination port unreachable.” To troubleshoot the issue, we used a network analyzer tool, tcpdump, and attempted to load the webpage again. We analyzed the packets, the error message is indicating that the UDP packet was undeliverable to port 53 of the DNS server for 203.0.113.2 domain is blocked or down. It could be a sign of a DOS attack on the company server, the DNS server is down, or the firewall blocked the connection. We recommend further investigation on the server. 

---

## Self-Assessment Score
Completed the Activity 1/1
Self-assessment: Passed (100% / 1 point)

## Artifacts
- Coursera activity: [https://www.coursera.org](https://www.coursera.org/learn/networks-and-network-security/assignment-submission/oKdjU/activity-analyze-network-layer-communication/attempt)
- GitHub writeup: [https://github.com/neilsteven1997/cybersecurity](https://github.com/neilsteven1997/cybersecurity/blob/main/writeups/coursera/google-cybersecurity-professional-certificate/course-3-connect-and-protect-networks-and-network-security/module-3-secure-against-network-intrusions_2026-03-06.md)

---

Last updated: April 02, 2026









