# Gobuster: The Basics

---

The TryHackMe room on Gobuster covers this open-source Golang tool for brute-force enumeration of web directories, DNS subdomains, 
virtual hosts, Amazon S3 buckets, and Google Cloud Storage buckets through targeted wordlists and response analysis. Professionals 
rely on it in penetration testing, bug bounty programs, and security assessments, slotting it between reconnaissance and scanning 
in the ethical hacking workflow. Enumeration involves listing accessible or hidden resources like directories, while brute force 
systematically tests every wordlist entry until valid matches emerge.

The lab environment uses an Ubuntu 20.04 virtual machine as the target web server, configured with multiple subdomains, virtual hosts, 
WordPress, and Joomla. Work occurs primarily from the pre-installed AttackBox, though local machines require a TryHackMe VPN connection
plus Gobuster setup following instructions in the official repository at https://github.com/OJ/gobuster. Start the web server machine,
wait roughly two minutes for boot, and launch the AttackBox separately. Because the setup relies on a local DNS server running on the
web server, edit /etc/resolv-dnsmasq on the AttackBox with sudo nano, insert nameserver MACHINE_IP as the first line, save via CTRL+O 
and ENTER, exit with CTRL+X, then restart the service using /etc/init.d/dnsmasq restart so the file reads with MACHINE_IP followed by 
IP_Address.

Running gobuster --help displays the core structure, listing available commands such as dir for directory and file enumeration, dns 
for subdomain enumeration, and vhost for virtual host enumeration, plus supporting modes for fuzzing, GCS, S3, and TFTP. Frequently 
used flags include -t or --threads to control concurrent requests (default 10, tunable by system resources), -w or --wordlist to supply
the dictionary file, --delay to space out requests and mimic normal traffic, --debug for error diagnosis, and -o or --output to log 
findings to disk. A combined example runs as gobuster dir -u "http://www.example.thm/" -w /usr/share/wordlists/dirb/small.txt -t 64.

The dir mode scans website structures and returns status codes to indicate accessible paths and files, proving valuable in tests 
because application directories often follow standard layouts such as the WordPress tree under html/wordpress with wp-admin, wp-content,
and wp-includes. The gobuster dir --help output lists essential flags covering most cases: -c or --cookies to attach session 
identifiers, -x or --extensions to hunt specific file types like .php or .js, -H or --headers for custom request headers, -k or 
--no-tls-validation to bypass certificate validation in self-signed test setups, -n or --no-status to suppress codes in output, -P or 
password and -U or --username for authenticated enumeration once credentials are obtained, -s or --status-codes to include chosen 
response codes or ranges, -b or --status-codes-blacklist to exclude unwanted ones (overriding -s), and -r or --followredirect to chase 
HTTP 301 or 302 redirects.

Directory mode always demands at minimum gobuster dir -u followed by the base URL (protocol required, hostname preferred over IP to 
correctly address virtual hosting) and -w for the wordlist; each entry appends to form requests without recursion into discovered paths.
Adding -r yields gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r, while 
-x extends it to gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js 
for both directories and matching file extensions.

DNS subdomain enumeration via dns mode becomes critical because subdomains may expose unpatched vulnerabilities absent from the primary
domain. The gobuster dns --help page highlights key flags: -c or --show-cname to reveal CNAME records, -i or --show-ips to display 
resolved addresses, -r or --resolver for a custom DNS server, and -d or --domain to set the target. The required syntax is gobuster 
dns -d example.thm -w /path/to/wordlist, demonstrated by gobuster dns -d example.thm -w 
/usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt, where each wordlist entry constructs a query appended to 
the domain.

Virtual host mode targets distinct sites sharing the same IP and server by manipulating the Host header, unlike dns mode which performs 
lookups. Flags from gobuster vhost --help include -u or --url for the base address, --append-domain to attach the domain to each 
wordlist entry, -m or --method to select the HTTP verb, --domain to populate the hostname portion, --exclude-length to drop responses 
by body size and eliminate false positives, and -r or --follow-redirect. A realistic command becomes gobuster vhost -u 
"http://MACHINE_IP" --domain example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain 
--exclude-length 250-320; the extra flags ensure correct Host header assembly (subdomain.example.thm) and filter typical 404 sizes, 
focusing on valid 200 responses. I found the --exclude-length option especially effective at cleaning output in lab environments 
lacking full DNS infrastructure.

---

| Description | Code/Command |
|-------------|--------------|
| Edit DNS resolver configuration on AttackBox | sudo nano /etc/resolv-dnsmasq |
| Restart Dnsmasq service after DNS edit | /etc/init.d/dnsmasq restart |
| Verify resolv-dnsmasq contents | cat /etc/resolv-dnsmasq |
| Display full Gobuster help | gobuster --help |
| Basic directory enumeration example with threads | gobuster dir -u "http://www.example.thm/" -w /usr/share/wordlists/dirb/small.txt -t 64 |
| Display dir mode specific help | gobuster dir --help |
| Directory enumeration with redirect following | gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r |
| Directory and file enumeration with extensions | gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js |
| Display dns mode specific help | gobuster dns --help |
| Subdomain enumeration example | gobuster dns -d example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt |
| Display vhost mode specific help | gobuster vhost --help |
| Virtual host enumeration with domain append and length filter | gobuster vhost -u "http://MACHINE_IP" --domain example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320 |
| View WordPress directory structure example | tree -L 3 -d |
| Sample GET request structure in vhost mode | GET / HTTP/1.1<br>Host: www.example.thm<br>User-Agent: gobuster/3.6<br>Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8<br>Accept-Language: en-US,en;q=0.5<br>Accept-Encoding: gzip, deflate<br>Connection: keep-alive |

---

**Extracted Tables**

**Common Gobuster Flags**

| Short Flag | Long Flag | Description |
|------------|-----------|-------------|
| -t | --threads | This flag configures the number of threads to use for the scan. Each of these threads sends out requests with a slight delay. The default number of threads is 10. This number may be slow when using large wordlists. You can increase or decrease the number of threads depending on the available system resources. |
| -w | --wordlist | The flag configures a wordlist to use for iterating. Each wordlist entry is attached to the URL you included in the command. |
|  | --delay | This flag defines the amount of time to wait between sending requests. Some web servers include mechanisms to detect enumeration by looking at how many requests are received in a certain period of time. We can increase the delay between subsequent requests to make it look like normal web traffic. |
|  | --debug | This flag helps us to troubleshoot when our command gives unexpected errors. |
| -o | --output | This flag writes the enumeration results to a file we choose. |

**Directory and File Enumeration Flags**

| Flag | Long Flag | Description |
|------|-----------|-------------|
| -c | --cookies | This flag configures a cookie to pass along each request, such as a session ID. |
| -x | --extensions | This flag specifies which file extensions you want to scan for. E.g., .php, .js |
| -H | --headers | This flag configures an entire header to pass along with each request. |
| -k | --no-tls-validation | This flag skips the process that checks the certificate when https is used. It often happens for CTF events or test rooms like the ones on THM a self-signed certificate is used. This causes an error during the TLS check. |
| -n | --no-status | You can set this flag when you don’t want to see status codes of each response received. This helps keep the output on the screen clear. |
| -P | password | You can set this flag together with the --username flag to execute authenticated requests. This is handy when you have obtained credentials from a user. |
| -s | --status-codes | With this flag, you can configure which status codes of the received responses you want to display, such as 200, or a range like 300-400. |
| -b | --status-codes-blacklist | This flag allows you to configure which status codes of the received responses you don’t want to display. Configuring this flag overrides the -s flag. |
| -U | --username | You can set this flag together with the --password flag to execute authenticated requests. This is handy when you have obtained credentials from a user. |
| -r | --followredirect | This flags configures Gobuster to follow the redirect that it received as a response to the sent request. A HTTP redirect status code (e.g., 301 or 302) is used to redirect the client to a different URL. |

**DNS Subdomain Enumeration Flags**

| Flag | Long Flag | Description |
|------|-----------|-------------|
| -c | --show-cname | Show CNAME Records (cannot be used with the -i flag). |
| -i | --show-ips | Including this flag shows IP addresses that the domain and subdomains resolve to. |
| -r | --resolver | This flag configures a custom DNS server to use for resolving. |
| -d | --domain | This flag configures the domain you want to enumerate. |

**Virtual Host Enumeration Flags**

| Short Flag | Long Flag | Description |
|------------|-----------|-------------|
| -u | --url | Specifies the base URL (target domain) for brute-forcing virtual hostnames. |
|  | --append-domain | Appends the base domain to each word in the wordlist (e.g., word.example.com). |
| -m | --method | Specifies the HTTP method to use for the requests (e.g., GET, POST). |
|  | --domain | Appends a domain to each wordlist entry to form a valid hostname (useful if not provided explicitly). |
|  | --exclude-length | Excludes results based on the length of the response body (useful to filter out unwanted responses). |
| -r | --follow-redirect | Follows HTTP redirects (useful for cases where subdomains may redirect). |

---

**Key Takeaways**
- Edit /etc/resolv-dnsmasq on the AttackBox to place nameserver MACHINE_IP as the first line, save the changes, exit the editor, then
  run /etc/init.d/dnsmasq restart to enable proper domain resolution in the local lab network.
- Launch dir mode with gobuster dir -u "http://target" -w wordlist.txt, optionally appending -r to follow redirects or -x .php,.js to
  include file extensions while noting that scans are non-recursive and require hostname in the URL for accurate virtual hosting.
- Execute dns mode enumeration using gobuster dns -d target.thm -w wordlist.txt to brute-force subdomains via DNS queries, optionally
  adding -i to show IP addresses.
- Run vhost mode with gobuster vhost -u "http://MACHINE_IP" -w wordlist.txt --domain example.thm --append-domain --exclude-length range
   to manipulate Host headers and filter false positives by response body size.
- Prefer hostname over raw IP in -u flags across modes to correctly target virtual hosts, adjust -t threads or --delay for performance
  versus evasion, and combine -U/--username with -P for authenticated scans when credentials exist.
- Distinguish modes clearly: dns performs DNS lookups on constructed FQDNs while vhost alters the Host header in direct HTTP requests
   to the same IP.

---





