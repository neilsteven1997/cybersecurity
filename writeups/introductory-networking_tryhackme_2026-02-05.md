# Introductory Networking

---

In my ongoing notes on foundational networking concepts relevant to cybersecurity analysis, I've revisited the essentials from an 
introductory perspective. The core objective here is understanding how data moves across systems, which directly ties into vulnerability 
assessments and threat modeling. Networking fundamentals boil down to models that abstract the complexity, starting with the OSI model, 
a seven-layer framework for conceptualizing data transmission. This model, while theoretical, clarifies the separation of concerns in 
network operations. At the top, the application layer interfaces with software to enable data exchange, handing off to the presentation 
layer for formatting, encryption, or compression into a standard form. The session layer then establishes and manages connections, 
ensuring unique sessions for concurrent communications without interference.

Continuing downward, the transport layer selects protocols like TCP for reliable, connection-oriented transfers where data is segmented 
and retransmitted if needed, or UDP for faster, less reliable scenarios prioritizing speed over accuracy. The network layer handles 
routing by determining optimal paths using logical addresses such as IPv4 formats familiar in home setups. The data link layer adds 
physical addressing via MAC addresses burned into network cards, formats the data for transmission, and verifies integrity on receipt. 
Finally, the physical layer deals with raw signal conversion and transmission over hardware.

Encapsulation wraps data with layer-specific headers as it descends, renaming it from generic data to segments or datagrams at 
transport, packets at network, frames at data link, and bits at physical. De-encapsulation reverses this on the receiving end, 
stripping headers upward. This standardization ensures interoperability across diverse devices. The TCP/IP model, predating OSI and 
underpinning real-world networks, condenses into four layers: application covering OSI's top three, transport mirroring OSI's, internet 
for routing akin to network, and network interface combining data link and physical. I find the OSI easier for initial breakdowns, 
but TCP/IP's practicality shines in actual implementations.

TCP/IP's protocols include TCP's three-way handshake for connections: a SYN packet initiates, the response combines SYN-ACK, and a 
final ACK confirms. This reliability contrasts UDP's fire-and-forget approach. Historically, TCP/IP standardized in 1982 by the U.S. 
Department of Defense to resolve vendor incompatibilities, with OSI later emerging as an ISO guide. In practice, tools like ping test 
connectivity via ICMP on the network layer, revealing IP addresses alongside reachability. Traceroute maps paths by showing intermediate
hops, useful for diagnosing routing issues. Whois queries domain registrars for registration details, often yielding renewal dates and 
nameserver info. Dig manually probes DNS resolution, querying recursive servers for IP mappings and TTL values indicating cache 
expiration in seconds.

DNS resolution starts locally with hosts files or caches, escalating to recursive servers, root name servers (originally 13 IPs), TLD 
servers by extension, and authoritative servers holding records. This hierarchy prevents needing to memorize IPs. I found this breakdown
particularly useful when tracing propagation delays in past investigations. For deeper dives, the Cisco Self-Study Guide by Steve 
McQuerry remains a solid reference, even if older editions are more accessible.

---

| Description | Code/Command |
|-------------|--------------|
| Test network connection to a target and resolve its IP | ping <target> |
| Example ping to Google | ping google.com |
| Map the route to a destination showing intermediate hops | traceroute <destination> |
| Query domain registration details | whois <domain> |
| Manually query a DNS server for domain information | dig <domain> @<dns-server-ip> |

---

Extracted Tables

| Layer | Name |
|-------|------|
| 7 | Application |
| 6 | Presentation |
| 5 | Session |
| 4 | Transport |
| 3 | Network |
| 2 | Data Link |
| 1 | Physical |

---

### Key Takeaways
- Topics covered: OSI Model, TCP/IP Model, practical model applications, basic networking tools.
- Mnemonic for OSI layers: Anxious Pale Shakespeare Treated Nervous Drunks Patiently (Application, Presentation, Session, Transport, Network, Data Link, Physical).
- OSI to TCP/IP mapping: Application (OSI 7-5) to Application, Transport (OSI 4) to Transport, Network (OSI 3) to Internet, Data Link and Physical (OSI 2-1) to Network Interface.
- Three-way handshake steps: Send SYN to initiate, receive SYN-ACK response, send ACK to confirm.
- DNS resolution sequence: Check local hosts file, then DNS cache; query recursive server; forward to root name server; redirect to TLD server; query authoritative name server for records.
- Further reading: Cisco Self-Study Guide by Steve McQuerry for comprehensive networking principles.

---


