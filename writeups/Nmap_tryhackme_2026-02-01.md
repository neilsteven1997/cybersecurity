# Nmap 

---

In enumeration during security audits, gathering details on targets proves essential prior to exploits. Assigned IPs demand initial mapping of active services, such as webservers or Active Directory controllers. Port scanning forms the core of this, where services listen on ports for connections. Ports enable concurrent requests or multiple services; browsers assign random local ports for tabs, while servers differentiate HTTP from HTTPS traffic via distinct ports. Connections link a client's random port to the server's listening one, like 49534 to 443 for web access. Computers offer 65535 ports, with standards including HTTP on 80, HTTPS on 443, NETBIOS on 139, and SMB on 445, though alterations occur in capture-the-flag scenarios, reinforcing need for thorough checks. Unknown open ports hinder attacks, so scans initiate processes. Nmap handles various scans by probing ports sequentially, classifying responses as open, closed, or filtered by firewalls. Service enumeration follows, often via Nmap. Its status as industry benchmark stems from unmatched features, enhanced by a scripting engine for vulnerability detection or exploitation. Grasping port scanning's role and Nmap's primacy in early reconnaissance stands out. Nmap operates via terminal on Windows or Linux, default in Kali and TryHackMe Attack Box. Invoke with nmap followed by switches; access help via nmap -h or man nmap. Primary scans include TCP Connect (-sT), SYN (-sS), UDP (-sU), plus less common Null (-sN), FIN (-sF), Xmas (-sX). These share goals but differ in mechanics, except UDP. ICMP scanning aids network overviews. TCP Connect scans rely on the three-way handshake: attacker sends SYN, target replies SYN/ACK if open, attacker sends ACK to complete. Closed ports prompt RST per RFC 9293. No response suggests filtering, though firewalls might send RST, complicating reads. An iptables command configures such rejection. SYN scans, dubbed half-open or stealth, abort after SYN/ACK with RST, avoiding full connections. This evades some legacy intrusion detection systems, skips app logging, and accelerates scans. Drawbacks include root privileges for raw packets, potential service instability. SYN defaults with sudo; without, TCP Connect runs. Alternatives exist via capabilities, though NSE scripts may falter. Closed or filtered ports mirror TCP Connect: RST for closed, drop or spoofed RST for filtered. UDP scans challenge due to stateless nature, favoring speed like in video. No handshake; open ports yield no response, labeled open|filtered; rare responses mark open. Closed ports return ICMP unreachable. Scans retry absent responses, slowing process—often 20 minutes for 1000 ports. Limit via --top-ports, scanning common UDP ports faster. Empty packets sent usually, but protocol payloads for known services improve accuracy. Less-used NULL, FIN, Xmas scans enhance stealth over SYN. NULL (-sN) sends flagless packets; RFC expects RST on closed. FIN (-sF) sets FIN flag; same expectation. Xmas (-sX) sets PSH, URG, FIN, resembling lit tree in Wireshark. Open ports silent, like UDP, yielding open|filtered; filtered via ICMP unreachable. Non-RFC behavior, like Windows or Cisco sending RST always, shows all closed. Aim bypasses SYN-dropping firewalls, though modern IDS detect. Black-box network mapping seeks active IPs. Ping sweep via -sn sends ICMP to ranges, marking responders alive, though inaccurate sometimes. Ranges use hyphen or CIDR, like 192.168.0.1-254 or /24. -sn skips port scans, uses ICMP echo or ARP locally with privileges, plus TCP SYN to 443, ACK or SYN to 80. Nmap Scripting Engine (NSE) boosts via Lua scripts for vuln scans to exploits, key in reconnaissance. Categories group scripts: safe avoids impact, intrusive risks effects, vuln checks weaknesses, exploit triggers them, auth bypasses logins like anonymous FTP, brute forces credentials, discovery queries services for network info like SNMP. A more exhaustive list can be found at https://nmap.org/book/nse-usage.html#nse-categories. Interact via --script for categories like vuln or safe, activating relevant ones on active services. Specific scripts via --script=<name>, like http-fileupload-exploiter; multiples comma-separated, as smb-enum-users,smb-enum-shares. Arguments via --script-args, e.g., for http-put uploading files. A full list of scripts and their corresponding arguments (along with example use cases) can be found at https://nmap.org/nsedoc/. Built-in help via nmap --script-help <name>. Find scripts via Nmap site or local /usr/share/nmap/scripts. Search script.db with grep, like for ftp; or ls with wildcards. Same for categories. Missing scripts? Update via sudo apt update && sudo apt install nmap; or manual download sudo wget -O /usr/share/nmap/scripts/<script-name>.nse https://svn.nmap.org/nmap/scripts/<script-name>.nse, then nmap --script-updatedb. Custom Lua scripts need updatedb too. Bypassing firewalls crucial; stealth scans help, but Windows defaults block ICMP, skipping scans unless -Pn assumes alive hosts, though slower if truly down. Local nets use ARP. Other evasion switches detailed at https://nmap.org/book/man-bypass-firewalls-ids.html; notable: -f fragments packets, --mtu <number> sets size (multiple of 8), --scan-delay <time>ms delays for unstable nets or evasion, --badsum sends invalid checksums to detect firewalls responding blindly. Scan <TARGET_IP> to apply techniques. Wrapped up Further Nmap exploration; Nmap's own highly extensive docs at https://nmap.org/docs.html serve well for reference.

| Description | Code/Command |
| --- | --- |
| Firewall configuration for RST response | iptables -I INPUT -p tcp --dport <port> -j REJECT --reject-with tcp-reset |
| UDP scan of top ports | nmap -sU --top-ports 20 <target> |
| Ping sweep with range | nmap -sn 192.168.0.1-254 |
| Ping sweep with CIDR | nmap -sn 192.168.0.0/24 |
| Script with arguments for file upload | nmap -p 80 --script http-put --script-args http-put.url='/dav/shell.php',http-put.file='./shell.php' |
| Search scripts database for term | grep "ftp" /usr/share/nmap/scripts/script.db |
| List scripts matching pattern | ls -l /usr/share/nmap/scripts/*ftp* |
| Search categories in database | grep "safe" /usr/share/nmap/scripts/script.db |
| Download script manually | sudo wget -O /usr/share/nmap/scripts/<script-name>.nse https://svn.nmap.org/nmap/scripts/<script-name>.nse |
| Update scripts database | nmap --script-updatedb |
| Run specific script | --script=http-fileupload-exploiter |
| Run multiple scripts | --script=smb-enum-users,smb-enum-shares |
| Run category scripts | --script=safe |
| Run vuln category | --script=vuln |
| Nmap help | nmap -h |
| Nmap manual | man nmap |
| Update Nmap package | sudo apt update && sudo apt install nmap |
| Script help | nmap --script-help <script-name> |
| TCP Connect scan | -sT |
| SYN scan | -sS |
| UDP scan | -sU |
| Null scan | -sN |
| FIN scan | -sF |
| Xmas scan | -sX |
| Ping sweep | -sn |
| Skip host discovery | -Pn |
| Fragment packets | -f |
| Set MTU | --mtu <number> |
| Scan delay | --scan-delay <time>ms |
| Bad checksum | --badsum |

Key Takeaways
- Basic Nmap scan types include TCP Connect scans (-sT), SYN half-open scans (-sS), and UDP scans (-sU).
- Less common port scan types encompass TCP Null scans (-sN), TCP FIN scans (-sF), and TCP Xmas scans (-sX).
- TCP three-way handshake proceeds as: attacker sends SYN-flagged packet, target responds with SYN/ACK if port open, attacker sends ACK to finish.
- For open port in SYN scan: send SYN, receive SYN/ACK, send RST to abort.
- Advantages of SYN scans: circumvents older intrusion detection systems seeking full handshakes, avoids logging by apps until connections establish, quicker without full handshake completion.
- Disadvantages of SYN scans: demands sudo for raw packet creation, risks destabilizing fragile services.
- For UDP open port: no response leads to open|filtered after retry; unusual UDP reply marks open.
- For UDP closed port: ICMP port unreachable response.
- For NULL, FIN, Xmas closed ports: expect RST response.
- For NULL, FIN, Xmas open ports: no response, resulting in open|filtered.
- Filtered ports in NULL, FIN, Xmas: often ICMP unreachable.
- NSE script categories: safe won't impact target, intrusive likely affects target, vuln scans vulnerabilities, exploit tries vulnerability exploitation, auth bypasses service authentication, brute forces service credentials, discovery queries services for network details.
- Activate NSE category: --script=<category>.
- Run single script: --script=<script-name>.
- Run multiples: --script=<script1>,<script2>.
- Provide arguments: --script-args <arg1>=<value>,<arg2>=<value>.
- Search installed scripts via database: grep <term> /usr/share/nmap/scripts/script.db.
- Search via listing: ls -l /usr/share/nmap/scripts/*<term>*.
- Install missing script: download then update database.
- Firewall evasion switches of note: -f for packet fragmentation, --mtu <number> for custom packet size control, --scan-delay <time>ms for inter-packet delays, --badsum for invalid checksums to probe presence.

