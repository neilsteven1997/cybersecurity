# Service Discovery and Reconnaissance on Target Hosts

---
Effective cybersecurity engagement requires systematic reconnaissance to identify exposed services on a target system. The standard 
methodology involves external port scanning followed by internal service enumeration. The initial steps target the host by its known 
IP address to discover listening ports, prioritizing common services like SSH (22) and HTTP (80). The approach expands from a basic 
check of the top 1000 ports to a full range scan (`-p-`), augmenting the scan with banner grabbing (`--script=banner`) to determine the 
exact service and version running. This process often reveals obscured services operating on non-standard ports, such as an FTP 
server running on port `21212` instead of the default `21`.

---
## Protocol-Specific Enumeration

Once open ports are identified, the next step is interacting with them using appropriate client tools. For known application protocols 
like `FTP`, the dedicated ftp client can be used to attempt anonymous login and file enumeration. For custom or unknown services (e.g., 
`TBFC maintd v0.2` on port `25251`), the universal utility Netcat (`nc`) provides a simple means to establish a raw TCP connection, 
allowing manual interaction with the service banner and issuing commands like `HELP` to discover available functionality. 
Reconnaissance is not limited to TCP; the entire range of UDP ports must also be scanned (`-sU`). Critical UDP services, such as DNS 
(port `53`), can then be queried using specific tools like dig to perform advanced lookups (e.g., querying for `TXT` records 
like `key3.tbfc.local`) to extract hidden information.

---
## On-Host Service and Database Discovery

Gaining internal access to the host allows for more comprehensive service discovery, bypassing external network firewalls. Once 
inside, the need for external port scanning is eliminated; the operating system itself can report active services. Commands 
like `ss -tunlp` (or `netstat` on older Linux systems) list all listening sockets, revealing services accessible externally (`0.0.0.0`) 
as well as those restricted to internal access (`127.0.0.1` or `localhost`). This frequently exposes services like a MySQL database 
running on port `3306`, which is often configured to allow unauthenticated access only from the localhost interface. With internal 
access, the mysql client can be executed directly on the host to enumerate databases and tables (e.g., `show tables;`), often 
leading to the discovery of sensitive data or flags stored within application tables.

---
## Commands

### Nmap Port Scanning

- `nmap MACHINE_IP`
Performs a basic scan of the top 1000 common TCP ports on the target.

- `nmap -p- --script=banner MACHINE_IP`
Scans all 65,535 TCP ports (-p-) and attempts to grab service banners for version identification.

- `nmap -sU MACHINE_IP`
Performs a scan of all 65,535 UDP ports (-sU).

### Service Interaction

- `ftp MACHINE_IP 21212`
Connects the FTP client to the target IP address on the non-standard port 21212.

- `ls`
(Within the FTP client) Lists files in the current directory.

- `get tbfc_qa_key1 -`
(Within the FTP client) Downloads the specified file and outputs content to standard output (-).

- `nc -v MACHINE_IP 25251`
Uses Netcat to establish a raw TCP connection to the custom service running on port 25251.

- `HELP`
(Input to the custom service via Netcat) Requests a list of available commands.

- `dig @MACHINE_IP TXT key3.tbfc.local +short`
Queries the DNS server at MACHINE_IP for the TXT record associated with the domain key3.tbfc.local.

### On-Host Enumeration (After Gaining Access)

- `ss -tunlp`
Lists all active sockets, showing TCP (t) and UDP (u) listening ports (l), processes 
(p), and suppressing name resolution (n).

- `mysql -D tbfcqa01 -e "show tables;"`
Connects to the local MySQL server, selects the tbfcqa01 database, and executes the SQL command to list all tables.

- `mysql -D tbfcqa01 -e "select * from flags;"`
Connects to the local MySQL server, selects the tbfcqa01 database, and executes the SQL command to retrieve all data from the flags table.

>[!Note]
>### You did it! Wareville is one step safer.
>The townsfolk are counting on you to keep Christmas secure.
>Head back to Wareville to continue your mission!






