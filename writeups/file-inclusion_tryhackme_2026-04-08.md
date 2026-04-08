#  File Inclusion

---

File inclusion vulnerabilities allow attackers to exploit local file inclusion (LFI), remote file inclusion (RFI), and directory 
traversal also known as path traversal within web applications that request files through URL parameters. These parameters appear as 
query strings attached to GET requests, for instance when a user supplies a filename such as userCV.pdf to display content like a CV. 
The flaws arise in languages such as PHP when functions receive unsanitized user input and pass it directly to file-handling operations
without validation.

Path traversal succeeds by manipulating the URL with sequences like ../ to escape the application's root directory and read files 
elsewhere on the server. A typical payload appended to a parameter such as get.?file= might read ../../../../etc/passwd on Linux 
systems. Windows targets use similar traversal to reach files like boot.ini or windows/win.ini. Common operating system files serve as
reliable test points during assessment because they exist in predictable locations and contain useful system details.

Local file inclusion exploits include, require, include_once, and require_once functions in PHP. One scenario passes a lang GET 
parameter directly into include, permitting arbitrary file reads such as /etc/passwd when no path prefix or validation exists. Another
prepends a languages/ directory inside the include call, forcing the payload to incorporate traversal to break out. Black-box testing 
relies on error output that reveals the full application path and any appended extensions like .php. The null byte %00 truncates the 
string after the desired filename, bypassing the extension. Keyword filters that strip ../ are evaded by repeating sequences such as 
....// or appending /.. to the filtered term. Forced directory requirements in the input are met by prefixing the traversal payload with
the mandatory path segment.

Remote file inclusion extends the attack surface when allow_url_fopen remains enabled, allowing inclusion of files hosted on an 
external server. An attacker places a malicious PHP script on their own system and injects its URL into the vulnerable parameter. The 
application fetches the remote file via a GET request and executes its contents locally, raising the impact to remote command execution,
sensitive information disclosure, cross-site scripting, or denial of service.

The attached virtual machine was deployed and reached through the TryHackMe network using OpenVPN or the AttackBox to interact with 
the application at http://<MACHINE_IP>/. Testing continued at the playground endpoint and the challenges index to apply the techniques 
and retrieve the flags. The error disclosure during invalid input proved especially useful for understanding the exact include 
construction without source code access.

| Description | Code/Command |
|-------------|--------------|
| PHP code demonstrating unsanitized GET parameter passed directly to include in first LFI scenario | <?PHP include($_GET["lang"]); ?> |
| PHP code demonstrating directory prefixed to unsanitized GET parameter inside include | <?PHP include("languages/". $_GET['lang']); ?> |
| Malicious PHP file hosted on attacker server for RFI demonstration | <?PHP echo "Hello THM"; ?> |

---

**Extracted Tables**

**Common OS Files**

| Location | Description |
|----------|-------------|
| /etc/issue | contains a message or system identification to be printed before the login prompt. |
| /etc/profile | controls system-wide default variables, such as Export variables, File creation mask (umask), Terminal types, Mail messages to indicate when new mail has arrived |
| /proc/version | specifies the version of the Linux kernel |
| etc/passwd | has all registered users that have access to a system |
| /etc/shadow | contains information about the system's users' passwords |
| /root/.bash_history | contains the history commands for root user |
| /var/log/dmessage | contains global system messages, including the messages that are logged during system startup |
| /var/mail/root | all emails for root user |
| /root/.ssh/id_rsa | Private keys for a root or any known valid user on the server |
| /var/log/apache2/access.log | the accessed requests for Apache web server |
| C:\boot.ini | contains the boot options for computers with firmware |

---

### Key Takeaways
- Keep system and services, including web application frameworks, updated with the latest version.
- Turn off errors to avoid leaking the path of the application and other potentially revealing information.
- A Web Application Firewall (WAF) is a good option to help mitigate web application attacks.
- Disable some features that cause file inclusion vulnerabilities if your web app doesn't need them, such as allow_url_fopen on and
  allow_url_include.
- Carefully analyze the web application and allow only protocols and wrappers that are in need.
- Never trust user input, and make sure to implement proper input validation against file inclusion.
- Implement whitelisting for file names and locations as well as blacklisting.
- Find an entry point that could be via GET, POST, COOKIE, or header values.
- Enter a valid input to see how the web server behaves.
- Enter invalid inputs, including special characters and common file names.
- Do not always trust what you supply in input forms is what you intended; use either a browser address bar or a tool such as Burpsuite.
- Look for errors while entering invalid input to disclose the current path of the web application; if there are no errors, then trial
  and error might be the best option.
- Understand the input validation and if there are any filters.
- Try injecting a valid entry to read sensitive files.

---







