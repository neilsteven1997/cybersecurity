# Activity: Analyze network layer communication
**Google Cybersecurity Professional Certificate – Course Activity**  
Neil Steven | April 12, 2026 | Portfolio: [https://github.com/neilsteven1997/cybersecurity](https://github.com/neilsteven1997/cybersecurity/tree/main/writeups/coursera/google-cybersecurity-professional-certificate/course-3-connect-and-protect-networks-and-network-security/module-3-activity-analyze-network-layer-communication)

## Executive Summary
>[!Note]
>_This scenario is based on a fictional company:_

Scenario

You are a cybersecurity analyst working at a company that specializes in providing IT services for clients. Several customers of clients reported that they were not able to access the client company website www.yummyrecipesforme.com, and saw the error “destination port unreachable” after waiting for the page to load. 

You are tasked with analyzing the situation and determining which network protocol was affected during this incident. To start, you attempt to visit the website and you also receive the error “destination port unreachable.” To troubleshoot the issue, you load your network analyzer tool, tcpdump, and attempt to load the webpage again. To load the webpage, your browser sends a query to a DNS server via the UDP protocol to retrieve the IP address for the website's domain name; this is part of the DNS protocol. Your browser then uses this IP address as the destination IP for sending an HTTPS request to the web server to display the webpage  The analyzer shows that when you send UDP packets to the DNS server, you receive ICMP packets containing the error message: “udp port 53 unreachable.” 

**Goal:** _Analyze DNS and ICMP traffic captured by a network protocol analyzer to identify the protocol involved in the cybersecurity incident and assess potential malicious activity._

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
Last updated: April 12, 2026






