# Web Enumeration

---

In the TryHackMe Web Enumeration room the focus fell on core tools for the enumeration phase against web targets, where strong skills 
prevent missing key details yet avoid unproductive tangents. The material assumes a connection to the TryHackMe network unless the 
AttackBox or a standard Kali instance is already in use, and it frames every example around the AttackBox environment. Manual techniques
came first because browser-based inspection often uncovers the golden ticket with minimal noise compared to automated scans right away. 
The developer console, opened with the F12 shortcut in Firefox or Chrome, exposes page source code, loaded assets, and client-side 
JavaScript debugging capabilities. Within the Inspector pane the full HTML becomes visible, frequently revealing developer comments 
wrapped in <!-- --> tags that never render on the live page.

Gobuster received dedicated coverage for complete beginners. Written in Go, the open-source low-level language created by a Google team
and contributors, it installs cleanly on Kali without any Go build steps through the single command sudo apt install gobuster. Global 
flags control threads, verbosity, progress display, banner noise, and output files, with the threads value typically raised to 64 for 
acceptable speed. In dir mode it brute-forces directory structures and files on websites, returning status codes that instantly 
indicate accessible paths and supporting extension searches for common file types. A typical run begins with gobuster dir followed by 
the -u base URL and -w wordlist, always specifying the protocol. Additional flags handle cookies, custom headers, TLS certificate 
bypass with -k for common CTF HTTPS issues, basic authentication, and status-code filtering. The dns mode shifts to subdomain 
brute-forcing because vulnerabilities sometimes hide on subdomains even when the main domain appears patched. The vhost mode targets 
virtual hosts sharing the same IP and server, which can conceal entirely separate sites not visible on standard port scans. Useful 
wordlists ship by default on Kali under paths such as /usr/share/wordlists/dirbuster/directory-list-2.3-*.txt and the various dirb 
sets, while the full SecLists repository maintained by Daniel Miessler at https://github.com/danielmiessler/SecLists installs via sudo
apt install seclists and supplies interchangeable lists across modes. The practical deployed an instance after a five-minute wait,
required appending the <MACHINE_IP> to /etc/hosts for <TARGET_DOMAIN>.thm and any discovered virtual hosts, and expected answers 
formatted as comma-separated lists.

WPScan targets WordPress installations and has remained a staple since its initial release in June 2011. It enumerates sensitive 
information disclosures around plugin and theme versions, discovers misconfigured paths such as wp-config files, tests weak password 
policies through brute force, identifies default files, and probes common web application firewall plugins. The tool ships 
pre-installed on recent Kali and Parrot releases; older versions pull it with sudo apt update && sudo apt install wpscan, while other 
distributions follow the developer guide at https://github.com/wpscanteam/wpscan/wiki/WPScan-User-Documentation. Updating the local 
vulnerability database with wpscan --update is mandatory before any scan. Enumeration commands leverage the --enumerate flag with t for
themes, which cross-references known asset locations such as wp-content/themes/twentytwentyone, p for plugins that rely on directory 
listings and README.txt metadata for version detection, and u for users pulled from post authors. Adding v cross-references discovered 
items against the WPVulnDB for known vulnerabilities, though API setup sits outside this room scope. Password attacks combine enumerated
usernames with lists such as rockyou.txt, and the --plugins-detection aggressive profile prevents missing items when a WAF blocks 
lighter traffic. A compact cheatsheet summarizes the short flags and full example syntax.

Nikto serves as a general-purpose open-source web server scanner first released in 2001. It works against any web application rather 
than WordPress alone and surfaces sensitive files, outdated server software, and common misconfigurations including directory listings,
CGI scripts, and missing XSS protections. Installation mirrors WPScan on Kali or follows the developer guide at 
https://github.com/sullo/nikto. The simplest scan runs with nikto -h and an IP or domain to pull advertised headers and probe for 
obvious issues such as Apache Tomcat defaults via favicon or example paths, plus enabled HTTP methods like PUT and DELETE. Input can 
pipe directly from Nmap grepable output for subnet ranges, or the -p flag lists multiple ports on a single host. Plugins selected via
-Plugin extend capability and include apacheusers for HTTP auth enumeration, cgi for exploitable scripts, robots for parsing robots.txt,
and dir_traversal for LFI attempts against files like /etc/passwd; the full list appears with --list-plugins or at 
https://github.com/sullo/nikto/wiki/Plugin-list. Verbosity increases through the -Display flag to show redirects, received cookies, or
errors. Tuning values narrow the scan to specific categories such as file upload, default files and misconfigurations, information 
disclosure, injection flaws, command execution, or SQL injection. Results export cleanly to text or HTML reports with the -o flag and 
appropriate extension. The final practical required deploying another instance, waiting five minutes, and scanning the exposed ports on
the <MACHINE_IP> to answer the associated questions.

---

**Code/Command Table**

| Description | Code/Command |
|-------------|--------------|
| Install Gobuster on Kali | sudo apt install gobuster |
| Basic Gobuster dir mode enumeration | gobuster dir -u http://<TARGET_IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt |
| Gobuster dir mode with file extensions | gobuster dir -u http://<TARGET_IP>/myfolder -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x.html,.css,.js |
| Gobuster dns mode for subdomains | gobuster dns -d <TARGET_DOMAIN>.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt |
| Gobuster vhost mode | gobuster vhost -u http://example.com -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt |
| Add main domain to /etc/hosts for Gobuster practical | echo "<MACHINE_IP> <TARGET_DOMAIN>.thm" >> /etc/hosts |
| Add discovered virtual host to /etc/hosts | echo "<MACHINE_IP> <SUBDOMAIN>.<TARGET_DOMAIN>.thm" >> /etc/hosts |
| Install WPScan on older Kali | sudo apt update && sudo apt install wpscan |
| Update WPScan vulnerability database | wpscan --update |
| WPScan enumerate themes | wpscan --url http://<TARGET_URL>/ --enumerate t |
| WPScan enumerate plugins | wpscan --url http://<TARGET_URL>/ --enumerate p |
| WPScan enumerate users | wpscan --url http://<TARGET_URL>/ --enumerate u |
| WPScan enumerate vulnerable plugins | wpscan --url http://<TARGET_URL>/ --enumerate vp |
| WPScan password brute-force attack | wpscan --url http://<TARGET_URL>/ --passwords rockyou.txt --usernames <USERNAME> |
| WPScan aggressive plugin detection | --plugins-detection aggressive |
| Basic Nikto scan | nikto -h <TARGET_IP> |
| Pipe Nmap output to Nikto for subnet | nmap -p80 172.16.0.0/24 -oG - \| nikto -h - |
| Nikto scan specific ports | nikto -h <TARGET_IP> -p 80,8000,8080 |
| Nikto with selected plugin | nikto -h <TARGET_IP> -Plugin apacheuser |
| Nikto output to HTML report | nikto -h http://<TARGET_IP> -o report.html |

---

**Extracted Tables**

Gobuster Global Flags  
| Flag | Long Flag | Description |  
|------|-----------|-------------|  
| -t | --threads | Number of concurrent threads (default 10) |  
| -v | --verbose | Verbose output |  
| -z | --no-progress | Don't display progress |  
| -q | --quiet | Don't print the banner and other noise |  
| -o | --output | Output file to write results to |

Gobuster dir Mode Other Useful Flags  
| Flag | Long Flag | Description |  
|------|-----------|-------------|  
| -c | --cookies | Cookies to use for requests |  
| -x | --extensions | File extension(s) to search for |  
| -H | --headers | Specify headers, -H 'Header1: val1' -H 'Header2: val2' |  
| -k | --no-tls-validation | Skip TLS certificate verification |  
| -n | --no-status | Don't print status codes |  
| -P | --password | Password for Basic Auth |  
| -s | --status-codes | Positive status codes |  
| -b | --status-codes-blacklist | Negative status codes |  
| -U | --username | Username for Basic Auth |

Gobuster dns Mode Other Useful Flags  
| Flag | Long Flag | Description |  
|------|-----------|-------------|  
| -c | --show-cname | Show CNAME Records (cannot be used with '-i' option) |  
| -i | --show-ips | Show IP Addresses |  
| -r | --resolver | Use custom server (format server.com or server.com:port) |

WPScan Summary Cheatsheet  
| Flag | Description | Full Example |  
|------|-------------|--------------|  
| p | Enumerate Plugins | --enumerate p |  
| t | Enumerate Themes | --enumerate t |  
| u | Enumerate Usernames | --enumerate u |  
| v | Use WPVulnDB to cross-reference for vulnerabilities. Example command looks for vulnerable plugins (p) | --enumerate vp |  
| aggressive | This is an aggressiveness profile for WPScan to use. | --plugins-detection aggressive |

Nikto Plugins  
| Plugin Name | Description |  
|-------------|-------------|  
| apacheusers | Attempt to enumerate Apache HTTP Authentication Users |  
| cgi | Look for CGI scripts that we may be able to exploit |  
| robots | Analyse the robots.txt file which dictates what files/folders we are able to navigate to |  
| dir_traversal | Attempt to use a directory traversal attack (i.e. LFI) to look for system files such as /etc/passwd on Linux (http://ip_address/application.php?view=../../../../../../../etc/passwd) |

Verbosing Nikto Scan  
| Argument | Description | Reasons for Use |  
|----------|-------------|----------------|  
| 1 | Show any redirects that are given by the web server | Web servers may want to relocate us to a specific file or directory, so we will need to adjust our scan accordingly for this |  
| 2 | Show any cookies received | Applications often use cookies as a means of storing data. For example, web servers use sessions, where e-commerce sites may store products in your basket as these cookies. Credentials can also be stored in cookies |  
| E | Output any errors | This will be useful for debugging if your scan is not returning the results that you expect |

Tuning Nikto Scan  
| Category Name | Description | Tuning Option |  
|---------------|-------------|---------------|  
| File Upload | Search for anything on the web server that may permit us to upload a file. This could be used to upload a reverse shell for an application to execute | 0 |  
| Misconfigurations / Default Files | Search for common files that are sensitive (and shouldn't be accessible such as configuration files) on the web server | 2 |  
| Information Disclosure | Gather information about the web server or application (i.e. version numbers, HTTP headers, or any information that may be useful to leverage in our attack later) | 3 |  
| Injection | Search for possible locations in which we can perform some kind of injection attack such as XSS or HTML | 4 |  
| Command Execution | Search for anything that permits us to execute OS commands (such as to spawn a shell) | 8 |  
| SQL Injection | Look for applications that have URL parameters that are vulnerable to SQL Injection | 9 |

---

### Key Takeaways
- Good enumeration skills prove vital in penetration testing because they reveal exactly what is being targeted without excessive noise
   that could alert defenses.  
- Manual browser inspection via the developer console and Inspector pane often yields faster, quieter results than immediate automated
  tooling.  
- Gobuster installation on Kali requires nothing beyond sudo apt install gobuster and works without building from source.  
- Dir mode enumerates directories and files while returning status codes and supports extension searches for configuration or
  application files.  
- DNS mode brute-forces subdomains because vulnerabilities may exist on subdomains even when the primary domain is secured.  
- Vhost mode identifies virtual hosts running on the same IP and server that standard port scans might miss.  
- Increase Gobuster threads to 64 for practical speed and always include the -k flag when invalid certificates appear during CTF HTTPS
  scans.  
- Kali default wordlists and the SecLists repository together cover nearly every enumeration scenario across modes.  
- Edit /etc/hosts with the <MACHINE_IP> and discovered subdomains or virtual hosts before browsing the target domain
  <TARGET_DOMAIN>.thm.  
- WPScan database must be updated with wpscan --update prior to any scan to ensure accurate theme, plugin, and vulnerability checks.  
- Theme enumeration uses known asset paths, plugin enumeration leverages directory listings plus README.txt metadata, and user
  enumeration pulls from post authors.  
- Combine enumeration output with password lists for brute-force attacks and apply aggressive detection profiles to bypass lightweight
  WAF blocks.  
- Nikto basic scans reveal server type, default paths, enabled methods, and misconfigurations on any web server.  
- Pipe Nmap grepable output or specify ports manually to scan ranges or multiple services efficiently.  
- Plugins such as apacheusers, cgi, robots, and dir_traversal extend targeted checks; verbosity and tuning flags refine output
  relevance.  
- Export Nikto results directly to HTML or text reports for offline analysis.  
- Recommended next steps include the Top 10 Walkthrough and EasyPeasyCTF for general web skills, the Blog room for WPScan practice, and
   Top 10, ToolsRUs, or EasyCTF for deeper Nikto usage.

---






