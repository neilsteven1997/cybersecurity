# Module 2: Network Operations 

---

In Module 2 on Network Operations I examined the core tools and protocols that security analysts use to defend organizational networks 
against exploitation of data in transit. Network protocols function as agreed-upon rules that dictate how devices structure and deliver 
data packets, acting essentially as instructions for handling information across global networks. I reviewed the three primary 
categories: communication protocols such as Transmission Control Protocol (TCP), which establishes reliable connections through a 
three-way handshake at the transport layer of the TCP/IP model; User Datagram Protocol (UDP), a connectionless alternative suited for 
speed-critical tasks like Domain Name System (DNS) queries on port 53; and Hypertext Transfer Protocol (HTTP) at the application layer 
on port 80. Security analysts must remain aware of vulnerabilities, for instance how malicious actors can abuse DNS to redirect traffic
to malware sites. Management protocols include Simple Network Management Protocol (SNMP) for device monitoring and configuration at the 
application layer, and Internet Control Message Protocol (ICMP) at the internet layer for error reporting and troubleshooting via 
commands like ping. Security protocols encompass Hypertext Transfer Protocol Secure (HTTPS) on port 443 with SSL/TLS encryption, and 
Secure File Transfer Protocol (SFTP) over SSH on TCP port 22 using Advanced Encryption Standard (AES) for file transfers, often with 
cloud storage.

Additional protocols I noted include Network Address Translation (NAT), which maps private IP addresses to a single public one through 
routers or firewalls at layers 2 and 3 of the TCP/IP model, alongside Dynamic Host Configuration Protocol (DHCP) for automated IP 
assignment on UDP ports 67 and 68. Address Resolution Protocol (ARP) translates IP addresses to permanent MAC addresses at the network 
access layer without assigned ports, storing mappings in device caches. Telnet provides remote access on TCP port 23 but transmits 
in clear text, while Secure Shell (SSH) offers encrypted alternatives on TCP port 22. Email protocols cover Post Office Protocol 
version 3 (POP3) for downloading messages on ports 110 or 995 with SSL/TLS, Internet Message Access Protocol (IMAP) for server-side 
access on ports 143 or 993 with TLS, and Simple Mail Transfer Protocol (SMTP) for routing on ports 25 or 587 with TLS. Firewalls 
leverage port numbers to filter traffic, and analysts should memorize these associations for interviews and daily work.

Wireless networking standards fall under IEEE 802.11, maintained by the Institute of Electrical and Electronics Engineers, with 
security evolving from Wired Equivalent Privacy (WEP) in 1999—now obsolete due to weak encryption—to Wi-Fi Protected Access (WPA) in 
2003 using Temporal Key Integrity Protocol (TKIP) and message integrity checks, then WPA2 in 2004 with Advanced Encryption Standard 
(AES) and Counter Mode Cipher Block Chaining Message Authentication Code Protocol (CCMP). WPA2 remains the current standard for most 
networks but shares KRACK vulnerabilities with its predecessor, prompting WPA3 in 2018 that introduces Simultaneous Authentication of 
Equals (SAE), stronger 128-bit encryption (or 192-bit in enterprise mode), and protection against handshake attacks. WPA2 and WPA3 
support personal mode for home use via shared passphrases and enterprise mode for centralized control in business environments.

Firewalls serve as network security devices that inspect traffic according to rules, available as hardware appliances, software 
installations, or cloud-based services known as Firewall as a Service. Stateful variants track session states for proactive threat 
filtering, outperforming stateless ones that rely solely on static rules, while next-generation firewalls add deep packet inspection, 
intrusion prevention, and integration with cloud threat intelligence. Virtual Private Networks (VPNs) encrypt data and mask IP 
addresses through encapsulation, creating secure tunnels over public networks to counter ISP visibility of requests and locations. 
Common protocols include WireGuard for its simplicity, speed, and open-source design suitable for both remote access and site-to-site 
setups, and IPSec for authenticated, encrypted tunnels favored in complex enterprise site-to-site deployments due to its maturity. 
Proxy servers act as intermediaries, with forward proxies regulating internal outbound access and reverse proxies shielding internal 
servers, both often incorporating Network Address Translation.

Security zones segment networks to isolate the uncontrolled external internet from internal assets, using a controlled zone that 
contains a demilitarized zone (DMZ) for public services such as web and DNS servers alongside a restricted zone for confidential data 
accessible only to privileged users. Subnetting divides address ranges into smaller logical groups via Classless Inter-Domain Routing 
(CIDR) notation, such as 198.51.100.0/24, improving efficiency and enabling security zones without additional public IP requests from 
internet service providers; you can try converting CIDR to IPv4 addresses and vice versa through an online conversion tool like 
https://www.ipaddressguide.com/cidr for practice. A security engineer at Google named Antara highlighted in discussion how daily tasks 
center on capturing and analyzing traffic to debug issues, collaborating across teams, and reusing established secure protocols rather
than reinventing solutions.

---

**Extracted Tables**

| Private IP Addresses          | Public IP Addresses                  |
|-------------------------------|--------------------------------------|
| Assigned by the router        | Assigned by ISP and IANA             |
| Unique only within private network | Unique address in global internet |
| No cost to use                | Costs to lease a public IP address   |
| Address ranges: 10.0.0.0-10.255.255.255, 172.16.0.0-172.31.255.255, 192.168.0.0-192.168.255.255 | Assignable address ranges: 1.0.0.0-9.255.255.255, 11.0.0.0-126.255.255.255, 128.0.0.0-172.15.255.255, 172.32.0.0-192.167.255.255, 192.169.0.0-233.255.255.255 |

| Protocol | Port |
|----------|------|
| DHCP | UDP port 67 (servers)<br>UDP port 68 (clients) |
| ARP | none |
| Telnet | TCP port 23 |
| SSH | TCP port 22 |
| POP3 | TCP/UDP port 110 (unencrypted)<br>TCP/UDP port 995 (encrypted, SSL/TLS) |
| IMAP | TCP port 143 (unencrypted)<br>TCP port 993 (encrypted, SSL/TLS) |
| SMTP | TCP/UDP Port 25 (unencrypted)<br>SMTPS: TCP/UDP port 587 (encrypted, TLS) |

---

### Key Takeaways
- Entry-level analysts should know fundamental protocols and their TCP/IP model layers plus associated ports to identify
  vulnerabilities and mitigate attacks.
- NAT, DHCP, ARP, Telnet, SSH, POP3, IMAP, and SMTP appear routinely in operations; understanding their ports and placement supports
  troubleshooting and firewall configuration.
- Wireless protocol history from WEP through WPA, WPA2, and WPA3 reveals specific vulnerabilities like KRACK and guides selection of
  up-to-date encryption for organizational networks.
- Subnetting via CIDR creates efficient internal segments and security zones without extra public IP allocations, enhancing performance
   and isolation.
- Firewalls range from stateless to stateful and next-generation types with deep packet inspection; proxy servers and VPNs with
  encapsulation add layered protection.
- VPN protocols such as WireGuard and IPSec define tunnel formation for remote access or site-to-site connections, with organizations
   increasingly combining VPN and SD-WAN in cloud models.
- Leverage established secure protocols and focus collaboration on unsolved challenges rather than rebuilding known solutions.

---

































































