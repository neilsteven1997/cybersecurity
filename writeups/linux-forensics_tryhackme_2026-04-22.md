# Linux Forensics

---

I spent some time today reviewing Linux forensics methodologies, a critical area of focus given how frequently Linux servers operate 
as public-facing enterprise entry points or employee endpoints. While Windows dominates the desktop landscape, the open-source, 
lightweight, and modular nature of Linux makes it ubiquitous across web servers, smartphones, and even vehicle entertainment systems. 
I specifically focused on the Ubuntu distribution for this session, though the core forensic concepts apply similarly to other major 
distributions like Redhat, ArchLinux, Open SUSE, Mint, CentOS, and Debian. To solidify my foundational understanding, I briefly 
referenced the TryHackMe (THM) Fundamentals 1, 2, and 3 rooms before diving into the specific artifacts. Since Linux treats everything
as a file, the core of my investigation involved locating and reading system configuration and log files to extract forensic data on a 
provided lab machine, which I accessed using the credentials username: [REDACTED_USER] and password: [REDACTED_PASSWORD].

I started by querying basic operating system and account details on the target machine. By reading the release configuration file 
located at /etc/os-release utilizing the cat utility and referencing its manual page via the man command, I confirmed the system was 
running Ubuntu 20.04.1 LTS (Focal Fossa). This file also conveniently provided official resources such as 
[https://www.ubuntu.com/](https://www.ubuntu.com/), [https://help.ubuntu.com/](https://help.ubuntu.com/), 
[https://bugs.launchpad.net/ubuntu/](https://bugs.launchpad.net/ubuntu/), and 
[https://www.ubuntu.com/legal/terms-and-policies/privacy-policy](https://www.ubuntu.com/legal/terms-and-policies/privacy-policy). 
Moving to user accounts, I parsed the /etc/passwd file, which structures data across seven colon-separated fields detailing usernames, 
password placeholders, User Identifiers (UID), Group Identifiers (GID), descriptions, home directories, and default shells. Formatting
this output with the column tool, I observed that standard, user-created accounts typically have UIDs of 1000 or greater. 
Cross-referencing this with /etc/group revealed specific group memberships, while the shadow file placeholder "x" indicated where the
actual password hashes securely reside. To determine who held administrative execution rights, I checked /etc/sudoers, a file strictly
requiring elevated root privileges to read.

Tracking login and authentication events proved crucial for establishing an execution timeline. I inspected /var/log/auth.log, parsing
it with utilities like tail, head, more, and less. This log captures all authentication attempts, including privilege escalation 
events where users execute commands via sudo, documenting the corresponding session opening and closing. For historical login data, I
analyzed the binary log files wtmp and btmp, which are located in the /var/log directory. Since standard text utilities like vim cannot
read these, I relied on the last utility to extract historical user sessions and failed login attempts. I then mapped the system's 
configuration footprint by examining /etc/hostname for the device name and /etc/timezone for temporal context, the latter providing a 
valuable indicator of the device's geographical location or operational window. Network configurations were extracted from 
/etc/network/interfaces. To understand the live networking state, I captured interface data via the ip utility, masking the target IP 
addresses [REDACTED_IP_1], [REDACTED_IP_2], and MAC address [REDACTED_MAC] for security. Live system analysis also involved utilizing
netstat to identify active network connections, established sessions, and listening ports, alongside reviewing running processes with 
the ps utility to spot behavioral anomalies. Furthermore, I reviewed /etc/hosts for local Domain Name System (DNS) assignments and 
/etc/resolv.conf for upstream DNS resolver configurations.

Investigating persistence mechanisms was my next priority, as malware authors frequently ensure their access survives a system reboot.
I reviewed the system-wide scheduled tasks in /etc/crontab, examining the execution intervals, designated users, and target scripts or 
binaries. I also verified background services set to execute on boot by listing the contents of the /etc/init.d directory. Another 
excellent persistence vector I scrutinized was the shell configuration scripts. I reviewed the target user's ~/.bashrc file, which acts
as a startup list of actions executing commands automatically whenever a non-login bash shell is spawned. I made a mental note to 
always check the system-wide equivalents at /etc/bash.bashrc and /etc/profile during an investigation. Finally, I gathered evidence of
execution. By filtering the authentication logs using the grep utility, I isolated the complete history of administrative commands 
run via sudo. For standard, non-elevated command execution, I dumped the ~/.bash_history file directly from the user's home directory.
This history revealed a sequential workflow of directory changes, archive extractions using unzip, and binary analysis utilizing the 
strings tool. This reinforced exactly how vital it is to check the command history of every user on the system, particularly the root
account, to reconstruct an attacker's complete footprint.

---

| Description | Code/Command |
| :--- | :--- |
| View tool manual pages | `man cat`, `man last`, `man ip`, `man netstat`, `man ps`, `man hosts` |
| View OS release info | `cat /etc/os-release` |
| OS Release Output | `NAME="Ubuntu"`<br>`VERSION="20.04.1 LTS (Focal Fossa)"`<br>`ID=ubuntu`<br>`ID_LIKE=debian`<br>`PRETTY_NAME="Ubuntu 20.04.1 LTS"`<br>`VERSION_ID="20.04"`<br>`HOME_URL="https://www.ubuntu.com/"`<br>`SUPPORT_URL="https://help.ubuntu.com/"`<br>`BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"`<br>`PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"`<br>`VERSION_CODENAME=focal`<br>`UBUNTU_CODENAME=focal` |
| Format user accounts file | `cat /etc/passwd \| column -t -s :` |
| Read system groups | `cat /etc/group` |
| Group Info Output | `root:x:0:`<br>`daemon:x:1:`<br>`bin:x:2:`<br>`sys:x:3:`<br>`adm:x:4:syslog,[REDACTED_USER]`<br>`tty:x:5:syslog` |
| Read sudoers configuration | `sudo cat /etc/sudoers` |
| Sudoers Output Snippet | `Defaults env_reset`<br>`Defaults mail_badpass`<br>`Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"`<br>`root ALL=(ALL:ALL) ALL`<br>`%admin ALL=(ALL) ALL`<br>`%sudo ALL=(ALL:ALL) ALL` |
| View historical logins | `sudo last -f /var/log/wtmp` |
| Historical Logins Output | `reboot system boot 5.4.0-1029-aws Tue Mar 29 17:28 still running`<br>`reboot system boot 5.4.0-1029-aws Tue Mar 29 04:46 - 15:52 (11:05)`<br>`reboot system boot 5.4.0-1029-aws Mon Mar 28 01:35 - 01:51 (1+00:16)`<br>`wtmp begins Mon Mar 28 01:35:10 2022` |
| Tail authentication logs | `cat /var/log/auth.log \| tail` |
| View system hostname | `cat /etc/hostname` |
| View system timezone | `cat /etc/timezone` (Output: `Etc/UTC`) |
| View network interfaces file | `cat /etc/network/interfaces` |
| Network Interfaces Output | `source /etc/network/interfaces.d/*`<br>`auto lo`<br>`iface lo inet loopback`<br>`auto eth0`<br>`iface eth0 inet dhcp` |
| View live IP addresses | `ip address show` |
| View active network connections | `netstat -natp` |
| View running processes | `ps aux` |
| Read local DNS hosts | `cat /etc/hosts` |
| View DNS resolver config | `cat /etc/resolv.conf` |
| View scheduled cron jobs | `cat /etc/crontab` |
| Cron Jobs Output Snippet | `SHELL=/bin/sh`<br>`PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin`<br>`17 * * * * root cd / && run-parts --report /etc/cron.hourly`<br>`25 6 * * * root test -x /usr/sbin/anacron \|\| ( cd / && run-parts --report /etc/cron.daily )` |
| List service startup scripts | `ls /etc/init.d/` |
| Read user bash profile | `cat ~/.bashrc` |
| Filter sudo execution history | `cat /var/log/auth.log* \| grep -i COMMAND \| tail` |
| Read user bash history | `cat ~/.bash_history` |
| Bash History Output | `cd Downloads/`<br>`ls`<br>`unzip PracticalMalwareAnalysis-Labs-master.zip`<br>`cd PracticalMalwareAnalysis-Labs-master/`<br>`ls`<br>`cd ..`<br>`ls`<br>`rm -rf sality/`<br>`ls`<br>`mkdir wannacry`<br>`mv Ransomware.WannaCry.zip wannacry/`<br>`cd wannacry/`<br>`unzip Ransomware.WannaCry.zip`<br>`cd ..`<br>`rm -rf wannacry/`<br>`ls`<br>`mkdir exmatter`<br>`mv 325ecd90ce19dd8d184ffe7dfb01b0dd02a77e9eabcb587f3738bcfbd3f832a1.7z exmatter/`<br>`cd exmatter/`<br>`strings -d 325ecd90ce19dd8d184ffe7dfb01b0dd02a77` |

---

### Extracted Tables

| Username | Password Info | UID | GID | Description | Home Directory | Default Shell |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| root | x | 0 | 0 | root | /root | /bin/bash |
| daemon | x | 1 | 1 | daemon | /usr/sbin | /usr/sbin/nologin |
| bin | x | 2 | 2 | bin | /bin | /usr/sbin/nologin |
| sys | x | 3 | 3 | sys | /dev | /usr/sbin/nologin |
| sync | x | 4 | 65534 | sync | /bin | /bin/sync |
| games | x | 5 | 60 | games | /usr/games | /usr/sbin/nologin |
| [REDACTED_USER] | x | 1000 | 1000 | [REDACTED_USER] | /home/[REDACTED_USER] | /bin/bash |
| pulse | x | 123 | 130 | PulseAudio daemon,,, | /var/run/pulse | /usr/sbin/nologin |
| tryhackme | x | 1001 | 1001 | tryhackme,,, | /home/tryhackme | /bin/bash |

*Table: System Accounts (/etc/passwd)*

| Interface | Link/Ether | IP Address (IPv4) | IP Address (IPv6) | State |
| :--- | :--- | :--- | :--- | :--- |
| lo | 00:00:00:00:00:00 | 127.0.0.1/8 | ::1/128 | UNKNOWN |
| eth0 | [REDACTED_MAC] | [REDACTED_IP_1]/16 | fe80::20:61ff:fef1:3ce9/64 | UP |

*Table: Live IP Configurations (ip address show)*

| Proto | Recv-Q | Send-Q | Local Address | Foreign Address | State | PID/Program name |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| tcp | 0 | 0 | 127.0.0.1:5901 | 0.0.0.0:* | LISTEN | 829/Xtigervnc |
| tcp | 0 | 0 | 0.0.0.0:80 | 0.0.0.0:* | LISTEN | - |
| tcp | 0 | 0 | 127.0.0.53:53 | 0.0.0.0:* | LISTEN | - |
| tcp | 0 | 0 | 0.0.0.0:22 | 0.0.0.0:* | LISTEN | - |
| tcp | 0 | 0 | 127.0.0.1:631 | 0.0.0.0:* | LISTEN | - |
| tcp | 0 | 0 | 127.0.0.1:60602 | 127.0.0.1:5901 | ESTABLISHED | - |
| tcp | 0 | 0 | [REDACTED_IP_1]:57432 | [REDACTED_IP_2]:443 | ESTABLISHED | - |
| tcp | 0 | 0 | [REDACTED_IP_1]:80 | [REDACTED_IP_3]:51934 | ESTABLISHED | - |
| tcp | 0 | 0 | 127.0.0.1:5901 | 127.0.0.1:60602 | ESTABLISHED | 829/Xtigervnc |
| tcp6 | 0 | 0 | ::1:5901 | :::* | LISTEN | 829/Xtigervnc |
| tcp6 | 0 | 0 | :::22 | :::* | LISTEN | - |
| tcp6 | 0 | 0 | ::1:631 | :::* | LISTEN | - |

*Table: Active Network Connections (netstat -natp)*

| USER | PID | %CPU | %MEM | VSZ | RSS | TTY | STAT | START | TIME | COMMAND |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| root | 729 | 0.0 | 0.0 | 7352 | 2212 | ttyS0 | Ss+ | 17:28 | 0:00 | /sbin/agetty -o -p -- \u --keep-baud 115200,38400,9600 ttyS0 vt220 |
| root | 738 | 0.0 | 0.0 | 5828 | 1844 | tty1 | Ss+ | 17:28 | 0:00 | /sbin/agetty -o -p -- \u --noclear tty1 linux |
| root | 755 | 0.0 | 1.5 | 272084 | 63736 | tty7 | Ssl+ | 17:28 | 0:00 | /usr/lib/xorg/Xorg -core :0 -seat seat0 -auth /var/run/lightdm/root/:0 -nolisten tcp vt7 -novtswitch |
| [REDACTED_USER] | 1672 | 0.0 | 0.1 | 5264 | 4588 | pts/0 | Ss | 17:29 | 0:00 | bash |
| [REDACTED_USER] | 1985 | 0.0 | 0.0 | 5892 | 2872 | pts/0 | R+ | 17:40 | 0:00 | ps au |

*Table: Running Processes (ps aux)*

---

### Key Takeaways
* Initiate forensic host enumeration by reading `/etc/os-release` to identify the precise distribution and version parameters.
* Cross-reference `/etc/passwd`, `/etc/group`, and `/etc/sudoers` to map local accounts, verify group memberships, and isolate accounts
   with elevated root execution privileges.
* Audit system authentication events in `/var/log/auth.log` and parse binary logs (`wtmp`, `btmp`) with the `last` utility to
  reconstruct user login timelines and failed access attempts.
* Extract static system identity footprints from `/etc/hostname` and `/etc/timezone` to contextualize the environment geographically
  and temporally.
* Assess networking parameters by inspecting static interface files (`/etc/network/interfaces`) and local routing/DNS configurations
  (`/etc/hosts`, `/etc/resolv.conf`).
* During live investigations, utilize `ip`, `netstat`, and `ps` to map the runtime state, actively listening ports, and anomalous
  process trees.
* Hunt for persistent threat mechanisms by carefully auditing scheduled tasks in `/etc/crontab`, background service binaries in
   `/etc/init.d/`, and shell-level persistence inside `~/.bashrc`, `/etc/bash.bashrc`, and `/etc/profile`.
* Establish concrete evidence of execution by filtering administrative commands within the authentication logs and diligently dumping
  the `~/.bash_history` for every user, ensuring the root directory is heavily prioritized.

---
