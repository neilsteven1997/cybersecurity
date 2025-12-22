# Nmap: The Basics

---

## Nmap serves as a versatile framework for network discovery and security auditing, addressing the fundamental requirements of 
identifying active devices and enumerating the services they host. While basic tools like ping or arp-scan offer rudimentary 
discovery, they are often hindered by ICMP filtering or the inability to scan beyond the local segment. Nmap overcomes these 
limitations through sophisticated scanning logic and the ability to target specific IP ranges, subnets, or hostnames. Effective 
use typically requires root or sudo privileges; without elevated access, the utility is restricted to fundamental operations 
such as TCP connect scans and basic ICMP echo requests.

## Host discovery represents the primary phase of an engagement, intended to map the active attack surface. When scanning a local 
network where the source and target share a data link, Nmap defaults to ARP requests, which provide the added benefit of 
identifying hardware vendors via MAC addresses. For remote networks separated by routers, Nmap utilizes a combination of ICMP 
echo requests, timestamp requests, and specific TCP probes to ports 80 and 443 to verify availability. A list scan is also 
available to catalog targets without initiating active traffic, providing a baseline for scoping without generating significant 
network noise.

## Service enumeration involves probing identified hosts to determine which TCP or UDP ports are listening. The TCP connect scan 
completes a full three-way handshake, making it reliable but highly visible in application logs. In contrast, the SYN scan—often 
termed a stealth scan—only initiates the first step of the handshake before terminating the connection with a reset packet, 
effectively identifying open ports while remaining relatively inconspicuous. UDP scanning presents different challenges, as the 
protocol is connectionless; Nmap relies on ICMP port unreachable messages to infer that a port is closed, which can be slower 
and less reliable due to rate limiting by the target's network stack.

## Advanced detection capabilities allow for the extraction of operating system details and specific service versions. OS detection 
utilizes fingerprinting techniques to analyze nuances in the network stack's response, providing an educated guess of the 
target's kernel version. Service version detection probes open ports to identify the specific software and version number, 
which is critical for vulnerability mapping. These features, along with traceroute, can be consolidated into a single aggressive 
scan. For environments where hosts may be configured to ignore discovery probes, forcing a scan by assuming all hosts are 
online ensures that no active services are missed.

## Scan performance is managed through timing templates and rate controls, ranging from paranoid to insane. Slower templates are 
designed to evade intrusion detection systems by increasing the delay between probes, while aggressive templates prioritize 
speed on reliable networks. Professionals can also specify the number of parallel probes or the packet rate to optimize the 
balance between speed and reliability. Output management is equally important, offering human-readable formats, XML for 
integration with other tools, and grepable formats for automated analysis. Enabling verbosity or debugging levels provides 
real-time insights into the scanning stages, ensuring that long-running tasks remain transparent.

---
| Description | Code/Command |
| --- | --- |
| Scan an IP range | `nmap 192.168.0.1-10` |
| Scan a specific subnet | `nmap 192.168.0.1/24` |
| Host discovery only (Ping scan) | `nmap -sn <target>` |
| List targets without scanning | `nmap -sL <target>` |
| TCP Connect scan | `nmap -sT <target>` |
| TCP SYN stealth scan | `nmap -sS <target>` |
| UDP scan | `nmap -sU <target>` |
| Scan 100 most common ports (Fast) | `nmap -F <target>` |
| Scan specific port range | `nmap -p1-1024 <target>` |
| Scan all 65,535 ports | `nmap -p- <target>` |
| Enable OS detection | `nmap -O <target>` |
| Enable service version detection | `nmap -sV <target>` |
| Aggressive scan (OS, version, scripts, traceroute) | `nmap -A <target>` |
| Treat all hosts as online (Skip discovery) | `nmap -Pn <target>` |
| Timing template (0=Paranoid, 5=Insane) | `nmap -T<0-5> <target>` |
| Set minimum/maximum packet rate | `nmap --min-rate <num> --max-rate <num>` |
| Set host timeout duration | `nmap --host-timeout <time> <target>` |
| Enable verbose output | `nmap -v` |
| Enable debugging output | `nmap -d` |
| Save output in all major formats | `nmap -oA <basename> <target>` |

---
### Key Takeaways - Use `sudo` or root privileges to unlock Nmap’s ability to craft raw packets for SYN and OS fingerprinting scans.
* Implement the `-sn` flag for initial discovery to minimize noise before moving to targeted port enumeration.
* Combine `-Pn` with port scans if a target is known to be active but blocks ICMP discovery probes.
* Adjust timing templates between `-T0` and `-T5` to balance the risk of detection against the requirement for scan speed.
* Utilize `-oA` to ensure scan results are archived in multiple formats for reporting and secondary tool processing.
* Limit port ranges with `-p` to reduce scan duration when only specific services like SSH or HTTP are of interest.

---



