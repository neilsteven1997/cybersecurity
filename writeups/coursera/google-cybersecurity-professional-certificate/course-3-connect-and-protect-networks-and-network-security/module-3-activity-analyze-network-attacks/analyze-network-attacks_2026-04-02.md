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

### Section 1: The type of attack that may have caused this network interruption

The potential explanation for the website's connection timeout error message is:
The logs show that client, 203.0.113.0:54770 tries to connect to HTTPS server 192.0.2.1:443. This event is likely to be a SYN Flood Attack. If the number of SYN requests is greater than the server’s available resource to handle the requests, then it becomes overwhelmed and unable to respond to the requests.  

### Section 2: How the attack is causing the website to malfunction

When website visitors try to establish a connection with the web server, a three-way handshake occurs using the TCP protocol. The three steps of the handshake:

1. SYN - the client sends request to the server. SYN means “Synchronize”.
2. SYN, ACK - the server received the request from the client and sends back SYN ACK. SYN ACK means “Synchronize Acknowledge.”
3. ACK - the client sends ACK to complete the three-way handshake. ACK means “Acknowledge.”

Attacker opens many SYN requests to the server without completing the handshake, exhausting the server. 

The logs indicate:
- Client 203.0.113.0 completed a handshake in the early phase
52	3.390692	203.0.113.0	192.0.2.1	TCP	54770->443 [SYN] Seq=0 Win=5792 Len=0...
53	3.441926	192.0.2.1	203.0.113.0	TCP	443->54770 [SYN, ACK] Seq=0 Win-5792 Len=120…
54	3.49316	203.0.113.0	192.0.2.1	TCP	54770->443 [ACK Seq=1 Win=5792 Len=0…

- Client 203.0.113.0 sends another request 
57	3.664863	203.0.113.0	192.0.2.1	TCP	54770->443 [SYN] Seq=0 Win=5792 Len=0…

- Other connections are failing because of the attack
73	6.230548	192.0.2.1	198.51.100.16	TCP	443->32641 [RST, ACK] Seq=0 Win-5792 Len=120...
77	7.330577	192.0.2.1	198.51.100.5	TCP	HTTP/1.1 504 Gateway Time-out (text/html)
80	7.380773	192.0.2.1	198.51.100.7	TCP	443->42584 [RST, ACK] Seq=1 Win-5792 Len=120…
85	7.680504	192.0.2.1	198.51.100.22	TCP	443->6345 [RST, ACK] Seq=1 Win=5792 Len=0…
92	13.967558	192.0.2.1	198.51.100.16	TCP	443->32641 [RST, ACK] Seq=1 Win-5792 Len=120...

- Multiple attacks followed
red	98	15.310554	203.0.113.0	192.0.2.1	TCP	54770->443 [SYN] Seq=0 Win=5792 Len=0...
red	99	15.734381	203.0.113.0	192.0.2.1	TCP	54770->443 [SYN] Seq=0 Win=5792 Len=0...
red	100	16.158208	192.0.2.1	203.0.113.0	TCP	443->54770 [RST, ACK] Seq=1 Win=5792 Len=0...
red	101	16.582035	203.0.113.0	192.0.2.1	TCP	54770->443 [SYN] Seq=0 Win=5792 Len=0...
red	102	17.005862	203.0.113.0	192.0.2.1	TCP	54770->443 [SYN] Seq=0 Win=5792 Len=0…

Suggested ways to secure the network, so this attack can be prevented in the future:
1. Block the Attacker’s IP   
2. Enable the SYN cookies - the kernel replies to SYNs with a cryptographic cookie instead of allocating a full connection slot until the ACK arrives.
3. Add rate-limiting rules - to limit new connections
4. Move the site behind Cloudflare - automatically absorb and filter SYN floods before they reach the server.
5. Tune backlog + monitoring for ongoing protection.

---

## Self-Assessment Score
Completed the Activity 1/1
Self-assessment: Passed (100% / 1 point)

## Artifacts
- Coursera activity: [https://www.coursera.org](https://www.coursera.org/learn/networks-and-network-security/assignment-submission/QHIX5/activity-analyze-network-attacks/attempt)
- GitHub writeup: [https://github.com/neilsteven1997/cybersecurity](https://github.com/neilsteven1997/cybersecurity/blob/main/writeups/coursera/google-cybersecurity-professional-certificate/course-3-connect-and-protect-networks-and-network-security/module-3-secure-against-network-intrusions_2026-03-06.md)

---

Last updated: April 02, 2026









