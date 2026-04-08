# Upload Vulnerabilities

---

In my notes on the TryHackMe Upload Vulnerabilities room I started with the getting started steps by deploying the target machine and 
then editing the local hosts file in an administrative session to enable domain name mapping without DNS reliance. On Linux or MacOS 
the file sits at /etc/hosts and requires sudo while on Windows it is at C:\Windows\System32\drivers\etc\hosts opened with Run as 
Administrator. I appended a single line at the end using the machine IP followed by the seven subdomains overwrite.uploadvulns.thm 
shell.uploadvulns.thm java.uploadvulns.thm annex.uploadvulns.thm magic.uploadvulns.thm jewel.uploadvulns.thm demo.uploadvulns.thm 
making sure to delete any prior matching entry first so only one line existed. After terminating the instance I removed the line 
entirely to avoid connectivity errors on redeployments since the IP changes and duplicate entries or active anonymising VPNs alongside 
the TryHackMe connection pack would otherwise break access.

The room examined how file uploads have become routine in web applications yet create severe risks when mishandled ranging from 
nuisance overwrites to full remote code execution or even defacement injection of malicious pages leading to cross-site scripting or 
worse. It specifically walked through overwriting existing files on a server uploading and executing shells bypassing client-side 
filtering bypassing various server-side filtering methods and fooling content type validation checks. Prior completion of the web 
enumeration section and the What the Shell room was advised for context.

Enumeration formed the foundation for any exploit beginning with source code review for client-side filtering directory bruteforcing 
with gobuster and request interception via Burp Suite while browser extensions such as Wappalyzer gave quick site insights. Once the 
handling logic was understood testing uploads revealed what passed or failed with error messages guiding refinements and tools like 
Burp Suite proving essential for iterative probing. Overwriting succeeded when no unique naming or permission safeguards existed as 
demonstrated by replacing images/spaniel.jpg on the demo site after confirming its path in the page source and uploading a matching 
filename.

Remote code execution elevated the impact by allowing arbitrary command execution on the server usually as a low-privileged web user. 
Webshells provided a lightweight option with a minimal PHP script that accepted a cmd GET parameter executed it via system and echoed 
the output. Reverse shells aimed higher using the pentestmonkey php-reverse-shell edited on line 49 to the TryHackMe tun0 IP then 
activated after starting a netcat listener. In both cases the file was placed in the uploads directory and triggered either directly or
through application routing depending on the setup.

Filtering defenses split into client-side scripts running in the browser and therefore trivial to bypass versus server-side code 
executing on the backend which demanded conforming payloads. Extension validation relied on blacklists or whitelists while file type 
checks covered MIME types attached in request headers such as image/jpeg and more robust magic numbers consisting of initial byte 
equences like 89 50 4E 47 0D 0A 1A 0A for PNG files. Additional layers included file length limits to prevent resource exhaustion name 
sanitisation to block bad characters such as null bytes or slashes and full content scanning though none of these were foolproof alone 
and were typically layered together. Client-side JavaScript whitelisting for example could be stripped before page load or the upload
request altered post-validation.

Server-side extension blacklisting was bypassed by trying alternative PHP-recognised extensions such as .phar or by crafting names 
like shell.jpg.php when the code only scanned for substring matches rather than strict last-period parsing. Magic number checks were 
defeated by prepending valid bytes such as FF D8 FF DB for JPEG to the start of a shell script first inserting placeholder characters 
then editing them in hexeditor and verifying the spoofed type with the file command before upload. Black-box testing followed a 
repeatable pattern of innocent uploads to map access paths and naming schemes followed by malicious attempts that exposed filter logic
through deliberate failures.

The room supplied an example methodology for black-box audits beginning with overall reconnaissance for languages frameworks and 
upload vectors via headers or extensions then inspecting the upload page source for client filters. Next came a benign upload to 
baseline naming access and directory locations potentially aided by gobuster with the -x switch for extension appending. Malicious 
uploads followed with analysis of rejection errors to infer blacklist versus whitelist behaviour magic number enforcement MIME 
filtering or length caps.

The optional challenge on jewel.uploadvulns.thm combined all prior techniques with multiple filters and a provided wordlist while 
noting that directory indexing might be disabled requiring direct URI access to the shell. In conclusion the room stressed reverting 
the hosts file changes immediately after use.

---

| Description | Code/Command |
|-------------|--------------|
| Line to append to hosts file for subdomain mapping | MACHINE_IP    overwrite.uploadvulns.thm shell.uploadvulns.thm java.uploadvulns.thm annex.uploadvulns.thm magic.uploadvulns.thm jewel.uploadvulns.thm demo.uploadvulns.thm |
| Minimal PHP webshell executing system commands from GET parameter | <?php<br>    echo system($_GET["cmd"]);<br>?> |
| Server-side PHP code example performing extension blacklist check with pathinfo | <?php<br>//Get the extension<br>$extension = pathinfo($_FILES["fileToUpload"]["name"])["extension"];<br>//Check the extension against the blacklist -- .php and .phtml<br>switch($extension){<br>case "php":<br>case "phtml":<br>case NULL:<br>$uploadFail = True;<br>break;<br>default:<br>$uploadFail = False;<br>}<br>?> |
| Pseudocode example of substring-based extension filter | ACCEPT FILE FROM THE USER -- SAVE FILENAME IN VARIABLE userInput<br>IF STRING ".jpg" IS IN VARIABLE userInput:<br>SAVE THE FILE<br>ELSE:<br>RETURN ERROR MESSAGE |
| Netcat listener command to catch reverse shell | nc -lvnp 1234 |
| Example curl command for direct file upload bypassing browser | curl -X POST -F "submit:<value>" -F "<file-parameter>:@<path-to-file>" <site> |
| Command to install gobuster on Kali if missing | sudo apt install gobuster |
| Command to remove last line from hosts file on Linux or MacOS | sudo sed -i '$d' /etc/hosts |
| PowerShell command to remove last line from hosts file on Windows | (GC C:\Windows\System32\drivers\etc\hosts \| select -Skiplast 1) \| SC C:\Windows\System32\drivers\etc\hosts |

---

### Key Takeaways
- Overwriting existing files on a server
- Uploading and executing shells on a server
- Bypassing client-side filtering
- Bypassing various kinds of server-side filtering
- Fooling content type validation checks
- Turn off Javascript in the browser provided the site does not require it for core functionality
- Intercept and modify the incoming page response with Burp Suite to strip out the Javascript filter before it loads
- Intercept the file upload request itself after the client-side filter has already accepted the payload then alter the extension and
  MIME type
- Send the file directly to the upload endpoint using a tool such as curl by replicating the observed parameters from a successful
  upload
- Begin with overall website reconnaissance using headers or extensions to identify languages frameworks and upload vectors
- Inspect the upload page source code to detect any client-side filters
- Perform an innocent file upload to establish baseline access paths naming schemes and directory locations potentially using gobuster
  with the -x switch for targeted extensions
- Attempt a malicious upload bypassing any identified client filters then analyse rejection error messages to infer server-side filter
  types through targeted tests for extension lists magic numbers MIME types or length limits

---






