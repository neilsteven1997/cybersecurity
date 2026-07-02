# FlareVM: Arsenal of Tools

---

The FLARE Team at FireEye developed FlareVM, which stands for Forensics, Logic Analysis, and Reverse Engineering. This environment 
functions as a specialized Windows-based virtual machine designed for malware analysis, digital forensics, incident response, and 
penetration testing. The ecosystem consolidates numerous tools across distinct operational categories, making it a valuable resource 
for breaking down binary control flows and analyzing malicious documents. To ensure safety, practicing inside this sandbox requires 
strict isolation protocols, as the sample dataset contains active malware that must never be executed or distributed outside the 
isolated host environment.

Initial triage of a suspicious binary frequently starts with static analysis to collect structural metadata without execution. Tools 
like PEStudio inspect the characteristics of a Portable Executable (PE) without triggering its payload. When evaluating anomalous 
files, such as a flagged Windows executable pretending to be the legitimate Windows Registry Editor (REGEDIT), PEStudio reveals core 
discrepancies. For instance, authentic registry utilities reside natively within the system directories rather than user download 
folders. Metadata analysis can also uncover regional language indicators or the complete absence of a rich header, which strongly 
implies the use of a packer or obfuscation layer meant to defeat signature-based detection. Furthermore, examining the Import Address 
Table (IAT) within the tool helps identify high-risk application programming interfaces associated with process spawning or 
cryptographic operations like AES encryption routines.

Complementing static analysis, string extraction tools like the FLARE Obfuscated String Solver (FLOSS) automate the recovery of 
hardcoded indicators of compromise, including embedded Internet Protocol (IP) addresses, Uniform Resource Locators (URLs) for 
command-and-control servers, registry paths, and configuration blocks. While FLOSS can output discovered strings into text files for 
documentation, its performance varies based on the underlying binary structure, particularly when processing managed code like .NET 
binaries that require different de-obfuscation techniques.

Dynamic analysis shifts the focus toward capturing real-time behavior during execution. By launching tools like Process Explorer and 
Process Monitor (Procmon), an analyst can track system modifications and active network connections side by side. Process Explorer 
maps parent-child process hierarchies, revealing whether a threat payload was spawned by standard system components or a user-initiated
shell. It also tracks specific process identifiers and exposes active network sockets. To manage the high volume of real-time event 
logs generated during execution, Procmon utilizes specific filter conditions to isolate process names, registry changes, and file 
system activity. Cross-referencing these discoveries with hex editors like HxD allows for direct validation of raw binary data, such
as identifying executable magic bytes or reviewing byte alignments via a data inspector interface.

---

### Extracted Tables

| Tool | Investigative Value |
| --- | --- |
| **Procmon** | Tracks real-time system activity, registry changes, and file activity for troubleshooting and forensic analysis. |
| **Process Explorer** | Exposes parent-child process relationships, active process paths, and loaded Dynamic Link Libraries (DLLs). |
| **HxD** | Conducts raw hexadecimal modification, binary inspection, and file structure verification. |
| **Wireshark** | Records, inspects, and analyzes network protocol traffic for anomalous connections. |
| **CFF Explorer** | Inspects Portable Executable (PE) structures, validates system files, and generates cryptographic hashes. |
| **PEStudio** | Performs static analysis to evaluate executable properties, blacklisted APIs, and metadata without running the file. |
| **FLOSS** | Extracts static strings and attempts advanced de-obfuscation of embedded text assets. |

---

### Code and Commands

| Description | Code/Command |
| --- | --- |
| Executing FLOSS to extract strings from a binary and saving the output to a text file | `FLOSS.exe .\windows.exe > windows.txt` |

---

### Key Takeaways

* **Core Tool Suites:** FlareVM groups its specialized software into clear operational segments, including Reverse Engineering &
Debugging (x64dbg, OllyDbg, Radare2, Binary Ninja, PEiD), Disassemblers & Decompilers (CFF Explorer, Hopper Disassembler, RetDec),
Forensics & Incident Response (Volatility, Rekall, FTK Imager), Network Analysis (Wireshark, Netcat), File Analysis (FileInsight, Hex
Fiend, HxD), and Scripting & Automation (Python, Empire).
* **Static Inspection Workflows:** Analysts should leverage PEStudio to review cryptographic hashes, check for structural anomalies
  like missing rich headers, and inspect the Import Address Table to discover blacklisted APIs.
* **Dynamic Monitoring Procedures:** When running a live sample, use Process Explorer to discover the Process ID (PID) and inspect
network properties. Simultaneously, set up custom filter rules in Process Monitor using specific string matches to isolate file system
modifications and network connections.
* **Hexadecimal Verification:** HxD provides direct visibility into raw binary files. Looking for early magic bytes like `4D 5A`
confirms a Portable Executable structure regardless of the file extension.

---



