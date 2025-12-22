# Tcpdump: The Basics

---

Network traffic analysis via `tcpdump` provides a granular perspective on protocol interactions that are typically obscured by high-level 
user interfaces. Developed in the late 1980s and written in C/C++, `tcpdump` utilizes the `libpcap` library to offer high-performance 
packet sniffing on Unix-like systems. This utility is essential for validating protocol behaviors, such as the TCP three-way handshake 
or ARP queries, which serve as the functional foundation of networking.

Effective packet capture requires precision in defining capture parameters. Analysts must identify the correct network interface, often 
verified using the `ip address show` command, before initiating a capture. Captures can be limited by packet count to prevent resource 
exhaustion or redirected to a `.pcap` file for retrospective analysis in tools like Wireshark. During live analysis, disabling DNS and 
port resolution accelerates the process and prevents the analyzer from generating its own traffic.

Filtering is indispensable for isolating specific "conversations" within high-volume traffic streams. Filtering expressions allow for 
the isolation of traffic by host, port, or protocol. For example, isolating traffic on port 53 reveals DNS queries and responses, while 
filtering for ICMP can identify diagnostic activities like ping or traceroute. Logical operators such as `and`, `or`, and `not` enable 
the construction of complex queries to target specific traffic profiles, such as encrypted SSH or web traffic.

Advanced filtering utilizes the `pcap-filter` syntax to evaluate specific bytes within a protocol header. This is achieved using 
the `proto[expr:size]` format, where the analyzer specifies an offset and byte length to inspect. This technique is particularly powerful 
for analyzing TCP flags, allowing an analyst to isolate packets with specific status bits set, such as SYN, ACK, or FIN. Binary 
operations—specifically AND (`&`), OR (`|`), and NOT (`!`)—provide the mathematical logic necessary to evaluate these header bits.

Output customization determines how captured data is interpreted. Options range from brief summaries for quick monitoring to detailed 
link-level headers that include MAC addresses. For payload inspection, data can be rendered in ASCII for plain-text protocols or in 
hexadecimal format for encrypted and binary data. Combining these views allows for a comprehensive assessment of both the protocol 
headers and the encapsulated data payload.

---
| Description | Code/Command |
| --- | --- |
| List available network interfaces | `ip a s` |
| Capture on specific interface | `tcpdump -i ens5` |
| Save capture to a file | `tcpdump -w traffic.pcap` |
| Read from a pcap file | `tcpdump -r traffic.pcap` |
| Limit capture to 5 packets | `tcpdump -c 5` |
| Disable IP and port resolution | `tcpdump -nn` |
| Verbose output levels | `-v`, `-vv`, `-vvv` |
| Filter by host | `tcpdump host 10.10.1.1` |
| Filter by source host | `tcpdump src host 10.10.1.1` |
| Filter by port | `tcpdump port 53` |
| Filter by protocol (e.g., ICMP) | `tcpdump icmp` |
| Logical AND filter | `tcpdump host 1.1.1.1 and tcp` |
| Filter for SYN flag only | `tcpdump "tcp[tcpflags] == tcp-syn"` |
| Filter for at least SYN set | `tcpdump "tcp[tcpflags] & tcp-syn != 0"` |
| Quick/Brief output | `tcpdump -q` |
| Print MAC addresses | `tcpdump -e` |
| Display data in ASCII | `tcpdump -A` |
| Display data in Hex | `tcpdump -xx` |
| Display Hex and ASCII | `tcpdump -X` |

---
What option can you add to your command to display addresses only in numeric format?
- `-n`

How many packets in traffic.pcap use the ICMP protocol?
- `26`

What is the IP address of the host that asked for the MAC address of 192.168.124.137?
- `192.168.124.148`

What hostname (subdomain) appears in the first DNS query?
- `mirrors.rockylinux.org`

How many packets have only the TCP Reset (RST) flag set?
- `57` 

What is the IP address of the host that sent packets larger than 15000 bytes?
- `185.117.80.53`


---
### Key Takeaways - Use `-i` to specify the interface; `any` listens across all active interfaces.
* Writing to a file with `-w` is standard practice for forensic preservation and secondary analysis.
* Disable name resolution with `-n` or `-nn` to improve performance and maintain a clean capture environment.
* Leverage logical operators (`and`, `or`, `not`) to narrow down traffic to specific investigative targets.
* Header bitmasking with `tcp[tcpflags]` enables precise identification of TCP session states.
* ASCII (`-A`) and Hex (`-X`) views are critical for determining if traffic contains cleartext credentials or encrypted payloads.
* Use `greater` or `less` filters to isolate packets based on their physical size on the wire.

---






