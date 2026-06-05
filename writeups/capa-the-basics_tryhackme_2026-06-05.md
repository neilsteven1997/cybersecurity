# CAPA: The Basics

---

CAPA serves as a static analysis tool developed by the FireEye Mandiant team for identifying capabilities within executable files such
as Portable Executables, ELF binaries, .NET modules, shellcode, and related artifacts. It applies rule sets encapsulating reverse 
engineering knowledge to detect behaviors including network communication, file manipulation, process injection, and anti-analysis 
techniques without executing the sample, thereby avoiding risks to the analysis environment. In practice, the tool processes files by 
loading rules and matching patterns, producing structured output with file hashes, analysis type, operating system context, 
architecture, and mapped behaviors.

Basic usage involves navigating to the working directory containing the binary and running capa.exe against the target, such as 
cryptbot.bin. Options like -v for verbose output and -vv for very verbose results provide increasing detail on matched rules, though 
processing time grows accordingly. Pre-generated reports including cryptbot.txt, cryptbot_vv.txt, and cryptbot_vv.json allow review 
without full re-execution. The output begins with file metadata such as MD5, SHA1, SHA256 hashes, static analysis indicator, Windows OS
target, PE format, i386 architecture, and file path.

Subsequent sections map findings to the MITRE ATT&CK framework, listing tactics like Defense Evasion with techniques such as Obfuscated
Files or Information (T1027) and sub-techniques, alongside Discovery, Execution, Impact, and Persistence entries. MAEC categorization 
tags the sample, for instance as a launcher exhibiting payload dropping or C2 connection behaviors. The Malware Behavior Catalogue (MBC)
details objectives like ANTI-BEHAVIORAL ANALYSIS with Lab Machine Detection (B0009), ANTI-STATIC ANALYSIS behaviors including 
Executable Code Obfuscation variants, COMMUNICATION via HTTP, DATA manipulations with Base64 and XOR, and host-interaction actions 
for file system and process operations.

Capabilities appear with corresponding namespaces that group related rules, covering anti-analysis for VM detection strings targeting 
VMware and VirtualBox, stackstring obfuscation, HTTP User-Agent and status code handling, encoding routines, PE section and export 
parsing, PowerShell execution, and scheduled task persistence via at or schtasks. Namespaces organize under top-level categories 
such as anti-analysis, communication, data-manipulation, executable, host-interaction, impact, linking, load-code, and persistence, 
each containing YAML rule files that define string matches, API calls, or structural features.

Very verbose mode with -vv, combined with JSON export via -j, facilitates deeper inspection of matched features within rules. The 
CAPA Web Explorer, accessible via local HTML file capa_web_explorer_offline.html or online version, visualizes these by uploading 
the JSON, highlighting specific strings like VMWare references or schtasks/create patterns under rule conditions using regex and 
logical operators. This reveals exact triggers such as string matches for anti-VM checks or scheduled task registration.

Redacted lab credentials include username: <redacted> and password: ******** for the Windows analysis machine. All analysis remained
static to maintain safety.

**Commands Table**

| Description | Code/Command |
|-------------|--------------|
| Run CAPA on binary | capa.exe .\cryptbot.bin |
| Show help | capa -h |
| Verbose output | capa.exe .\cryptbot.bin -v |
| Very verbose output | capa.exe .\cryptbot.bin -vv |
| View pre-processed report | Get-Content .\cryptbot.txt |
| JSON very verbose export | capa -j -vv .\cryptbot.bin > cryptbot_vv.json |

---

**Extracted Tables**

**File Metadata**
| Field | Value |
|-------|-------|
| md5 | 3b9d26d2e7433749f2c32edb13a2b0a2 |
| sha1 | 969437df8f4ad08542ce8fc9831fc49a7765b7c5 |
| sha256 | ae7bc6b6f6ecb206a7b957e4bb86e0d11845c5b2d9f7a00a482bef63b567ce4c |
| analysis | static |
| os | windows |
| format | pe |
| arch | i386 |
| path | /home/strategos/Room-CAPA/cryptbot.bin |

**ATT&CK Mappings** (partial)
| ATT&CK Tactic | ATT&CK Technique |
|---------------|------------------|
| DEFENSE EVASION | Obfuscated Files or Information [T1027] |
| DEFENSE EVASION | Obfuscated Files or Information::Indicator Removal from Tools [T1027.005] |
| DISCOVERY | File and Directory Discovery [T1083] |
| EXECUTION | Command and Scripting Interpreter::PowerShell [T1059.001] |
| PERSISTENCE | Scheduled Task/Job::Scheduled Task [T1053.005] |

**MBC Behaviors** (selected)
| MBC Objective | MBC Behavior |
|---------------|--------------|
| ANTI-BEHAVIORAL ANALYSIS | Lab Machine Detection [B0009] |
| DATA | Encode Data::Base64 [C0026.001] |
| FILE SYSTEM | Create Directory [C0046] |
| PROCESS | Create Process [C0017] |

**Capabilities and Namespaces** (selected)
| Capability | Namespace |
|------------|-----------|
| reference anti-VM strings | anti-analysis/anti-vm/vm-detection |
| contain obfuscated stackstrings (2 matches) | anti-analysis/obfuscation/string/stackstring |
| reference HTTP User-Agent string | communication/http |
| encode data using XOR | data-manipulation/encoding/xor |
| create directory | host-interaction/file-system/create |
| schedule task via schtasks | persistence/scheduled-tasks |

---

## Key Takeaway
- Launch CAPA from the dedicated directory using capa.exe followed by the target binary path to initiate static rule matching.
- Employ -v for standard verbose results showing ATT&CK, MAEC, MBC, and capability summaries; use -vv for rule feature details including
  matched strings and logic.
- Redirect very verbose JSON output with -j flag for import into CAPA Web Explorer to browse matched conditions interactively.
- Interpret general metadata block first for file identification and environment details before reviewing mapped tactics, behaviors,
  and namespaces.
- Cross-reference capabilities against their YAML rule files under top-level namespaces like anti-analysis or host-interaction to 
  understand exact detection logic such as regex string presence or API sequences.
- Utilize pre-processed reports in the capa directory to bypass long runtimes during initial familiarization.

---





