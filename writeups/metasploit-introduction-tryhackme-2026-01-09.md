# Metasploit Introduction 

---
Metasploit remains the industry standard for modular exploitation, facilitating everything from initial reconnaissance to 
post-exploitation. While the Pro version offers a graphical interface and automation, the open-source Framework is the primary 
tool for command-line driven penetration testing. The architecture relies on several core components: the vulnerability 
(the underlying flaw), the exploit (the code leveraging that flaw), and the payload (the code executed on the target). 
Framework modules are categorized by function, including Auxiliary for scanning and fuzzing, Encoders and Evasion for 
attempting to bypass security controls, and Post for activities following successful compromise. Payloads are further 
divided into singles, which are self-contained, and staged variants that use a small stager to download a larger stage 
component. I find that identifying the payload type is straightforward in the console; a forward slash typically indicates 
a staged payload, while an underscore denotes an inline or single payload.

The primary interface for these tools is msfconsole. Operating effectively within this environment requires constant 
awareness of the prompt state, as commands are context-dependent. A standard system prompt handles OS-level tasks, whereas 
the `msf6 >` prompt indicates the global framework environment. Once a module is selected via the `use` command, the prompt 
changes to a context-specific state, allowing for the configuration of variables. Following a successful exploit, the prompt 
may transition to a Meterpreter agent or a native target shell like `C:\Windows\system32>`. Failing to recognize these 
transitions often leads to syntax errors, such as trying to run Metasploit-specific commands within a raw target shell.

Variable management is handled through the `set` and `setg` commands. Most modules require an `RHOSTS` (remote host) and 
`RPORT` (remote port), while reverse payloads require an `LHOST` (local host) and `LPORT`. For large-scale testing, `RHOSTS` 
supports CIDR notation or file-based target lists via `file:/path/to/targets.txt`. While `set` limits a variable to the 
current module context, `setg` applies it globally, which is highly efficient when moving from an auxiliary scanner to an 
exploit. The `search` command, coupled with filters like `type:` or `platform:`, is the most effective way to navigate the 
thousands of available modules. Exploits are assigned a reliability rank, detailed in the official documentation 
at [https://github.com/rapid7/metasploit-framework/wiki/Exploit-Ranking](https://github.com/rapid7/metasploit-framework/wiki/Exploit-Ranking), 
which describes the likelihood of a module succeeding without crashing the target.

Execution is triggered by the `exploit` or `run` commands. I frequently use the `-z` flag to immediately background a 
session upon creation, which keeps the console clear for additional tasks. Many modules also support a `check` function, 
providing a safer way to confirm a target’s vulnerability without actually executing a payload. When dealing with 
vulnerabilities like MS17-010 (EternalBlue), as referenced in the Microsoft Security Bulletin at [https://docs.microsoft.com/en-us/security-updates/SecurityBulletins/2017/MS17-010](https://docs.microsoft.com/en-us/security-updates/SecurityBulletins/2017/MS17-010), 
the technical details often involve complex memory corruption, such as the `Srv!SrvOs2FeaToNt` buffer overflow. These exploits 
are archived with CVE references like CVE-2017-0143 (found at [https://cvedetails.com/cve/CVE-2017-0143/](https://cvedetails.com/cve/CVE-2017-0143/)) 
and are often based on research from groups like RiskSense-Ops ([https://github.com/RiskSense-Ops/MS17-010](https://github.com/RiskSense-Ops/MS17-010)). 
Active connections are managed through the `sessions` command, where the `-i` flag allows for re-interacting with backgrounded 
shells.

---
| Description | Code/Command |
| --- | --- |
| Initialize the Metasploit console | `msfconsole` |
| Search for modules using a keyword or CVE | `search ms17-010` |
| Filter search results by module type | `search type:auxiliary telnet` |
| Select a module by path or index | `use exploit/windows/smb/ms17_010_eternalblue` |
| Display configuration options for the current context | `show options` |
| Show all payloads compatible with the current exploit | `show payloads` |
| Set a local variable for the current module | `set RHOSTS 10.10.10.5` |
| Set a global variable for all modules | `setg RHOSTS 10.10.10.5` |
| Clear all configured variables in a module | `unset all` |
| Execute the current module and background the session | `exploit -z` |
| Verify vulnerability without full exploitation | `check` |
| Return to the global prompt from a module | `back` |
| List all active background sessions | `sessions` |
| Interact with a specific session ID | `sessions -i 1` |
| Move an active Meterpreter session to the background | `background` |

---
### Key Takeaways

* Metasploit Framework is the open-source, CLI-based version of the tool, distinct from the Pro GUI.
* The system is divided into modules: Auxiliary, Exploit, Payload, Encoder, Evasion, NOP, and Post.
* Payloads are either "singles" (standalone) or "staged" (stager + stage), identifiable by naming syntax.
* Five distinct prompt states exist: OS shell, msfconsole global, module context, Meterpreter, and target shell.
* Global variables (`setg`) persist across different modules, whereas `set` is context-specific.
* `exploit -z` is the preferred method for maintaining control over multiple incoming sessions.
* Session interaction is managed via the `sessions -i` command to move between backgrounded targets.

---
