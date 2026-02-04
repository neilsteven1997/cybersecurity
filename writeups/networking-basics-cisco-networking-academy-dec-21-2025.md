# Networking Basics

---
In my continued notes on foundational networking as it pertains to cybersecurity roles, I revisited a quiz example where a security 
pro teams up with IT to roll out software for pinpointing threats and system flaws. I first pegged it as an audit process, but it's
actually about setting up monitoring tools, which underscores the hands-on side of analyst work in spotting issues before they escalate,
much like configuring detection in a live environment.

The material kicks off with overviews of various network setups, ways data gets sent, metrics for capacity and flow rates, and recaps of
linked systems. It then covers roles like endpoints and central units, hardware essentials, links to service providers, and ties those
together. Wireless configurations follow, along with device pairings on the go, wrapping with thoughts on those arrangements. Practical
building of domestic networks includes core ideas, home tech choices, non-cabled norms, router installation, and an overall synthesis. A
skills check on assembling compact setups comes next. Rules for exchanges delve into agreed methods, uniform guidelines, layered
frameworks for interactions, and a consolidation of those principles.

Carriers for signals get examined through available kinds and a quick close. The entry-level tier looks at data wrapping with specific
frame formats, its core duties, and key points. Core addressing protocols explain the role of version 4 identifiers, their layout, and
protocol essentials. Version 4 handling and division covers single, mass, and group transmissions, identifier varieties, logical splits,
and a review. Next-gen formats address prior version limits, detailing new schemes and structure rules with an end note. Automated
assignment contrasts fixed setups, details version 4 setups, and concludes on flexible methods.

Weaving in basics, terms like bandwidth measure data handling speed to avoid bottlenecks in threat monitoring, while throughput tracks
actual transfer rates for efficient scans. Clients request services from servers in typical setups, and components such as routers
direct traffic between segments, switches connect devices locally, and modems link to external lines. ISP options include DSL for phone-
based access or fiber for high-speed, each with security implications like encryption needs. Wireless standards like 802.11 govern Wi-Fi
security via WPA protocols to thwart eavesdropping. Communication models like OSI layers help dissect vulnerabilities from physical
media up to applications. Ethernet frames encapsulate data with headers for error checking, crucial in access layers where switches
operate. IPv4 addresses, 32-bit dotted decimals, enable unicast for one-to-one, broadcast for all, multicast for groups, with types like
private for internal use to mask from public exposure. Subnetting segments networks for better control and isolation against breaches.
IPv6 expands addressing to 128 bits, fixing exhaustion issues. DHCP dynamically assigns IPs, reducing manual errors in large
environments.

I found these particularly handy in triaging incidents, where understanding encapsulation spots tampering or segmentation limits lateral
movement by attackers.

---

| Description | Code/Command |
|-------------|--------------|
| Test connectivity to a host by sending echo requests | ping [destination IP or domain] |
| Display network interface configuration, including IP address, subnet mask, and gateway | ipconfig (Windows) or ifconfig (Linux/macOS) |
| Trace the route packets take to a destination, showing hops and latency | tracert [destination] (Windows) or traceroute [destination] (Linux/macOS) |
| Show active network connections, listening ports, and routing tables | netstat -a |
| Query DNS servers to resolve domain names to IP addresses | nslookup [domain] |
| Display or manipulate ARP cache for mapping IPs to MAC addresses | arp -a |
| View or configure network interfaces and routes | ip addr show or ip route |
| Test reachability and measure round-trip time | ping -c 4 [destination] (for count-limited) |
| Flush DNS cache to resolve resolution issues | ipconfig /flushdns (Windows) |
| Display detailed statistics on network interfaces | netstat -s |

---

## Key Takeaways
- Network types include LAN for local connectivity and WAN for broader links, aiding in scoping security perimeters.
- Data transmission involves packets as basic units carrying information across nodes.
- Bandwidth defines maximum data capacity, while throughput measures real performance under load.
- Clients and servers form core interactions, with clients initiating requests.
- Network components encompass routers for inter-network routing, switches for local traffic, and hubs for basic connections.
- ISP connectivity options like cable, DSL, or satellite each carry distinct bandwidth and security profiles.
- Wireless networks rely on standards such as 802.11 for signal propagation and encryption.
- Mobile device connectivity integrates cellular or Wi-Fi for on-the-move access.
- Home network basics stress secure router placement and firmware updates.
- Network technologies in the home include wired Ethernet for stability and wireless for convenience.
- Wireless standards evolve to boost speed and range while enhancing security.
- Set up a home router involves configuring SSID, passwords, and port forwarding cautiously.
- Communication protocols like TCP ensure reliable delivery, UDP for faster but less assured.
- Communication standards set by bodies like IEEE ensure interoperability.
- Network communication models such as OSI or TCP/IP guide layered analysis for vulnerabilities.
- Network media types range from copper cables for short distances to fiber optics for long-haul.
- Encapsulation wraps data in headers and trailers for transmission integrity.
- Ethernet frame structures include source/destination MAC, payload, and checksum.
- The access layer handles device connections via switches and basic forwarding.
- Purpose of an IPv4 address is unique device identification on IP networks.
- IPv4 address structure uses 32 bits in four octets, like 192.168.1.1.
- IPv4 unicast targets single devices, broadcast all in subnet, multicast specific groups.
- Types of IPv4 addresses include public for internet-facing and private for internal.
- Network segmentation via subnets improves efficiency and contains threats.
- IPv4 issues stem from address depletion, prompting conservation techniques.
- IPv6 addressing uses 128-bit hexadecimal for vast scalability.
- Static addressing assigns fixed IPs manually, dynamic via protocols for automation.
- DHCPv4 configuration automates IP leasing, including options like DNS servers.

---








