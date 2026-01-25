# Detecting Web Shells

---

This entry consolidates my notes on detecting web shells, focusing on practical indicators across logs, file systems, and network 
traffic. Web shells remain a reliable attacker technique for both initial access and persistence, typically deployed through weak 
file upload handling or misconfiguration. Once present, they enable remote command execution and support later stages of the kill 
chain including reconnaissance, privilege escalation, lateral movement, data exfiltration, and cleanup. The material emphasized how 
even minimal scripts, often placed in common upload paths, are sufficient to sustain attacker control when monitoring is weak.

The walkthrough refreshed the definition of a web shell as a malicious server-side program uploaded to a web server, commonly written 
in languages already supported by the environment such as PHP or ASPX. I noted the example of a PHP shell residing in an uploads 
directory on a target host, illustrating how trivial placement blends with legitimate content. The abuse of built-in execution 
functions stood out as a recurring theme, particularly PHP functions like shell_exec, exec, system, and passthru, which are legitimate 
features but become dangerous when user input is passed unchecked.

Real-world cases reinforced the relevance. Hafnium leveraged the ProxyLogon vulnerabilities to deploy ASPX web shells on Microsoft 
Exchange servers, placing them in aspnet_client directories and even modifying legitimate files under Exchange frontend paths. Their 
post-compromise activity followed a predictable pattern of command execution, credential dumping, persistence via new accounts, 
lateral movement, and exfiltration. Conti ransomware operators exploited the same class of Exchange weakness, uploading an 
aspnetclient_log.aspx shell and quickly staging a secondary shell before enumerating domain infrastructure. These examples aligned 
closely with the theoretical attack flow described earlier.

Log-based detection formed the core of the defensive approach. Web server access logs from Apache and Nginx provide early signals 
when interpreted correctly. Repeated GET requests probing upload locations, followed by POST activity, often precede shell deployment. 
Persistent interaction with a single script through GET or POST can indicate active shell usage. Particular attention was given to 
HTTP methods that are rare in normal browsing, such as PUT or DELETE, and to anomalous response codes like repeated 404s interspersed
with 200 OK responses. User-Agent analysis remains valuable, especially when shortened, outdated, or tool-based strings such as 
curl or wget appear, and when source IPs fall outside expected internal ranges.

Query strings were highlighted as a high-signal artifact. Long or suspicious parameters containing command keywords, or strings 
encoded in Base64, often accompany shell interaction. The absence of a referrer header can add context, although I reminded myself 
that this alone is not conclusive given modern privacy controls. A composite example tied together multiple weak signals: an 
external IP, off-hours timestamp, POST to a PHP file with a query string, no referrer, and a non-browser User-Agent. None are 
definitive alone, but together they form a credible indicator.

Host-level auditing complements web logs. Auditd on Linux systems can record file creation, modification, and execution events when 
properly configured. Searching audit logs with ausearch for a dedicated rule key can confirm whether a web server process wrote or 
executed a suspicious script and under which user context. Correlating these records with web access logs allows reconstruction of 
the attack chain, for example linking a suspicious POST request to a creat or execve system call performed by the www-data user. 
This correlation is far more persuasive than isolated log entries.

The notes stressed the value of centralization through Security Information and Event Management platforms. Aggregating web logs, 
audit logs, and other telemetry simplifies correlation and supports targeted queries focused on web shell behavior. Efficiency and 
context were the main advantages called out, especially when dealing with diverse log sources.

Detection does not end with logs. File system analysis remains critical because shells must exist somewhere on disk unless content 
is database-backed, as with platforms like WordPress or Django. Default document roots for Apache and Nginx were identified, along 
with commonly guessed upload directories and even temporary paths such as /tmp when permissions are lax. File naming anomalies, 
executable extensions, and double extensions are reliable heuristics. I found the emphasis on temporal analysis useful, searching 
for recently modified scripts within specific windows to narrow scope.

Network traffic analysis provides the deepest visibility when packet captures are available. The same indicators used in logs apply 
at the packet level, with the added benefit of inspecting payloads directly. Observing a shell’s source code within a POST upload 
or seeing encoded commands in transit conclusively validates exploitation. Wireshark HTTP filters were referenced as practical tools 
for isolating these patterns, reinforcing how traffic analysis can mirror and extend log-based findings.

The investigation scenario tied everything together through a simulated WordPress compromise. Reviewing Apache access logs for 
repeated requests to PHP files, unusual response patterns, and abnormal User-Agents formed the initial triage. Command-line 
filtering with grep was encouraged to accelerate pattern discovery, gradually building an attacker profile that informs deeper 
searches. Credentials and target details were provided for hands-on access, but from a defensive perspective the exercise underscored 
disciplined log review and hypothesis-driven analysis.

Overall, the material reinforced that web shell detection is most effective when approached holistically. No single artifact is 
decisive, but consistency across web logs, audit trails, file system changes, and network traffic provides clarity. I found the 
integration of real-world threat actor behavior with practical analysis steps particularly useful for translating theory into 
repeatable investigative practice.

---

| Description                                                                | Code/Command                                                                      |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| Searching audit logs for events matching a specific auditd rule key        | ausearch -k web_shell                                                             |
| Finding PHP files modified within a defined time window under the web root | find /var/www -type f -name "*.php" -newerct "2025-07-01" ! -newerct "2025-08-01" |
| Recursively searching WordPress content for suspicious function usage      | grep -r "eval(" wp-content                                                        |
| Filtering Apache access logs for 404 responses                             | cat /var/log/apache2/access.log | grep "404"                                      |
| Accessing a deployed web shell via HTTP with a URL-encoded command         | curl "http://<TARGET_IP>:8080/files/awebshell.php?cmd=<URL_ENCODED_COMMAND>"      |
| Wireshark filter to isolate HTTP requests by method                        | http.request.method == "METHOD"                                                   |
| Wireshark filter to locate requests targeting PHP resources                | http.request.uri contains ".php"                                                  |
| Wireshark filter to inspect User-Agent values                              | http.user_agent                                                                   |

---

### Extracted Tables

| Request Method | Normal Usage               | Possible Abuse                                 |
| -------------- | -------------------------- | ---------------------------------------------- |
| GET            | Retrieve a resource        | Reconnaissance or interacting with a web shell |
| POST           | Submit data to the server  | Uploading or interacting with a web shell      |
| PUT            | Upload or replace a file   | Uploading a web shell                          |
| DELETE         | Remove a resource          | Cleanup actions                                |
| OPTIONS        | Discover supported methods | Reconnaissance                                 |
| HEAD           | Retrieve headers only      | File detection                                 |

| Field             | Value       |
| ----------------- | ----------- |
| Username          | <redacted>  |
| Password          | ********    |
| IP address        | <TARGET_IP> |
| Connection method | SSH         |

---

### Key Takeaways

* Web shells act as both an initial access vector and a persistence mechanism by abusing legitimate server-side functionality and
  insecure file handling.
* Common deployment relies on weak file upload validation, predictable directories, and executable scripting support already present
  on the server.
* High-signal web log indicators include repeated probing requests, anomalous HTTP methods, suspicious User-Agents, odd response code
  patterns, and encoded or command-oriented query strings.
* Auditd logs on Linux systems can confirm file creation, modification, or execution linked to suspicious web requests when correlated
  properly.
* File system analysis should focus on unexpected scripts, recent modifications, executable extensions, and deceptive naming such as
  double extensions.
* Network traffic inspection can conclusively reveal shell uploads and command execution by exposing payload contents and encoded
  attacker input.
* Effective detection depends on correlating multiple data sources rather than relying on any single indicator.
* System execution functions in PHP, such as shell_exec(), exec(), system(), and passthru(), can be abused to gain command execution.

---



