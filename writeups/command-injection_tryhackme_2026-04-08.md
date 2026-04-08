# Command Injection

---

Command injection abuses an application to execute operating system commands under the same privileges as the running process. If a web
server operates as the user joe, injected commands inherit joe's permissions and access rights. The flaw is frequently termed remote 
code execution or RCE because it permits direct remote interaction with the system, such as reading files containing sensitive data. 
It appeared among the top ten vulnerabilities in Contrast Security’s AppSec intelligence report in 2019 and continues to rank in the 
top ten proposed by the framework for web applications.

The root cause lies in applications that feed user input directly into operating system functions across languages like PHP, Python, 
and NodeJS. One case passes a $title value from a search field into grep to query songtitle.txt for song matches within an MP3 
directory. An attacker appends extra commands to access restricted files instead. The same pattern appears in a Python Flask setup that
imports subprocess and executes any command supplied through a defined web route.

Exploitation succeeds by chaining commands with shell operators such as semicolon, ampersand, and double ampersand. Detection splits 
into blind command injection, where no output appears and success is verified through application behavior like response delays from 
ping or sleep payloads or by redirecting output to a file later read with cat, and verbose command injection, where command results 
display directly on the page. A practical test submits payloads via curl, for instance combining a legitimate search term with whoami.

Linux payloads include whoami to reveal the running user, ls to enumerate the current directory for configuration files or tokens, ping 
and sleep to induce measurable delays in blind testing, and nc to spawn a reverse shell for navigation and privilege escalation. 
Windows equivalents cover whoami, dir, ping, and timeout for identical purposes.

Remediation centers on restricting dangerous PHP functions that invoke the shell, specifically exec, passthru, and system, while 
enforcing input sanitization to accept only expected formats such as numbers through the filter_input function. Filters that strip 
characters can be bypassed by supplying hexadecimal representations that the application still processes to the same effect.

The attached machine was deployed and opened in split-screen view. Various payloads were tested against the visible web application to
confirm injection and retrieve the flag contents from /home/tryhackme/flag.txt, with the referenced cheat sheet consulted for further 
options when initial attempts required refinement.

| Description | Code/Command |
|-------------|--------------|
| Curl example combining legitimate search input with whoami injection to test command injection | curl http://vulnerable.app/process.php%3Fsearch%3DThe%20Beatles%3B%20whoami |

---

**Extracted Tables**

**Method**

| Method | Description |
|--------|-------------|
| Blind | This type of injection is where there is no direct output from the application when testing payloads. You will have to investigate the behaviours of the application to determine whether or not your payload was successful. |
| Verbose | This type of injection is where there is direct feedback from the application once you have tested a payload. For example, running the whoami command to see what user the application is running under. The web application will output the username on the page directly. |

---

**Linux**

| Payload | Description |
|---------|-------------|
| whoami | See what user the application is running under. |
| ls | List the contents of the current directory. You may be able to find files such as configuration files, environment files (tokens and application keys), and many more valuable things. |
| ping | This command will invoke the application to hang. This will be useful in testing an application for blind command injection. |
| sleep | This is another useful payload in testing an application for blind command injection, where the machine does not have ping installed. |
| nc | Netcat can be used to spawn a reverse shell onto the vulnerable application. You can use this foothold to navigate around the target machine for other services, files, or potential means of escalating privileges. |

---

**Windows**

| Payload | Description |
|---------|-------------|
| whoami | See what user the application is running under. |
| dir | List the contents of the current directory. You may be able to find files such as configuration files, environment files (tokens and application keys), and many more valuable things. |
| ping | This command will invoke the application to hang. This will be useful in testing an application for blind . |
| timeout | This command will also invoke the application to hang. It is also useful for testing an application for blind if the ping command is not installed. |

---

### Key Takeaways
- How to discover the vulnerability
- How to test and exploit this vulnerability using payloads designed for different operating systems
- How to prevent this vulnerability in an application
- Applying your learning by performing command injection in a practical application

---


