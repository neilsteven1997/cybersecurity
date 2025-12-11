# Attacks Against Encrypted Files 

---
## The primary vector for compromising encrypted files protected by modern cryptography 
involves circumventing the algorithm's strength by focusing on the associated password's inherent weakness. Attackers systematically 
execute two core cracking methodologies: the dictionary attack and the mask attack. Dictionary attacks are fundamentally efficient, 
leveraging massive, predefined wordlists, such as common breach credentials and predictable patterns like `password123` substitutions, 
to exploit the human tendency toward weak password selection. This initial, high-speed phase is successful against a significant 
portion of targets.

---
## When initial wordlist attempts fail, the adversary pivots to brute-force techniques, often refined into mask attacks. 
While traditional brute-forcing systematically checks every character combination—a process that becomes exponentially time-prohibitive
with complexity—mask attacks strategically limit the search space. By defining a specific format, such as three lowercase letters 
followed by two digits (`?l?l?l?d?d`), the attacker strikes a necessary balance between thoroughness and speed, particularly when 
target password structure knowledge is available. High-performance cracking operations maximize efficiency by utilizing specialized, 
GPU-accelerated tools like Hashcat, which dramatically outperforms CPU-bound alternatives for numerous hashing algorithms, alongside 
flexible frameworks like John the Ripper, often pre-processing encrypted files using utilities such as `zip2john` or `pdf2john`.

---
## Effective defense against these offline attacks relies entirely on monitoring endpoint telemetry, 
as failed logon services remain untouched, rendering traditional perimeter-based detection useless. Crucial indicators of 
compromise (IOCs) are process creation events captured via mechanisms like Sysmon Event ID 1 on Windows, or auditd on Linux, 
specifically searching for well-known cracking binaries (`john`, `hashcat`, `fcrackzip`, `pdfcrack`) and distinctive command-line 
arguments referencing `--wordlist`, `--mask`, or specific wordlist files like `rockyou.txt`. Furthermore, the resource-intensive 
nature of accelerated cracking produces specific GPU artifacts, including sustained, high GPU utilization and power draw, 
detectable via tools like `nvidia-smi`, or the loading of graphics libraries such as `nvcuda.dll`. Network-level detection, while 
less central, remains relevant for initial activity, capturing bulk downloads of large wordlist repositories or package 
installations of cracking utilities. Detection rules must also monitor unusual file I/O patterns, such as repeated reads of 
large text files functioning as wordlists.

---
Upon confirmed detection, the mandated response playbook requires immediate host isolation to prevent further activity, 
even in lab environments. Security analysts must triage the situation by capturing critical artifacts, including a process list, 
process memory dumps, `nvidia-smi` output, and the encrypted file itself. Preservation efforts must meticulously secure the attacker’s 
working directory, all wordlists, hash files, and shell history for forensic review. The investigation subsequently focuses on 
identifying the specific files decrypted and tracing any resulting post-exploitation actions, such as lateral movement or data 
exfiltration. Following authorization review and escalation to the Incident Response team, final remediation involves rotating 
all affected keys and passwords, enforcing Multi-Factor Authentication (MFA), and ensuring correct placement of security tools 
into approved, monitored environments to conclude the incident lifecycle.



