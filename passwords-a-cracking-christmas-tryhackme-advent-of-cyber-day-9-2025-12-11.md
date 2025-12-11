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

---
## The exploitation process, typically referred to as an Offline Password Cracking attack 
when targeting password-protected documents or stolen hash databases, follows a predictable, multi-stage workflow designed to recover 
weak passwords without triggering defensive measures like account lockouts.

1. Data Acquisition and Target Preparation
The attack begins with a prerequisite step of data acquisition, where the attacker must first obtain the password-protected file or
a copy of the hashed password database from a compromised system (e.g., a PDF, ZIP, or network credential hash).
- File Analysis: The file's format is analyzed to determine the encryption method and select the correct hash-extraction utility.
- Hash Extraction: Specialized tools (such as `zip2john` or `pdf2john`) are used to convert the protected file into a hash format that 
password-cracking software can read. This hash represents the encrypted password and is saved as a new file (e.g., `hash.txt`).

Tool,Command,Purpose

PDF Hash Extraction,`python pdf2john.py 1file.pdf > 1file.hash`,Used to extract the password hash from a protected PDF document 
(1file.pdf) and save it to a hash file (1file.hash).

ZIP Hash Extraction,`zip2john.pl archive.zip > zip.hash`,(Implied utility) Extracts the password hash from a protected ZIP archive.

Windows Hash Extraction,`samdump2` or similar tools,(Utility used for credential dumping) Extracts hashes from Windows files like 
the Security Account Manager (SAM) database.

>[!Note]
>The output is typically redirected (>) to a new file (e.g., 1file.hash).

2. Tooling and Hardware Optimization
Standard, high-performance cracking tools are selected, such as John the Ripper or Hashcat. The attacker utilizes hardware
acceleration, specifically leveraging the parallel processing power of modern GPUs, which allows the cracking process to check
billions of password guesses per second, drastically reducing the time required for the attack.

Tool,Command,Purpose

Start Cracking,`john /root/Desktop/file.hash`,Runs the password cracking process (usually Brute-Force or Dictionary Attack) against 
the specified hash file.

Show Cracked Password,`john --show /root/Desktop/file.hash`,Displays any passwords that the cracking tool has successfully recovered 
and stored.

Use Hashcat,`hashcat -m <hash_mode> -a <attack_mode> hash.txt wordlist.txt`,"(Implied structure) The primary command structure 
for Hashcat, where you specify the hash type (-m), attack type (-a, e.g., dictionary, brute-force), the hash file, and the wordlist."

>[!Note]
>The actual commands used vary widely depending on the target system (Windows, Linux, specific application file), the type of 
>hash, and the cracking tool chosen.

3. Attack Phase 1: Dictionary Attack
The most efficient technique is executed first. A Dictionary Attack uses massive, pre-compiled wordlists (often containing millions
of common passwords, phrases, and previously leaked credentials) and tests each entry against the target hash. This high-probability
approach quickly cracks any password based on common words or easily guessed patterns.

4. Attack Phase 2: Hybrid and Rule-Based Attack
If the dictionary attack fails, the process pivots to a more targeted approach. A Hybrid Attack or Rule-Based Attack takes dictionary
words and applies common transformation rules to them. These rules mimic human behavior, such as capitalizing the first letter,
appending common numbers or years (e.g., `'password'` becoming `'Password1'` or `'Password2024'`), or substituting characters (e.g., `'s'`
with `'$'`).

5. Attack Phase 3: Mask and Brute-Force
As a final, exhaustive measure, the attacker resorts to Mask Attacks or Brute-Force Attacks.
- Mask Attack: This systematically tests all combinations within a limited, defined character set and length (a "mask") based on
  known password patterns (e.g., two letters followed by three digits).
- Brute-Force Attack: This method attempts every possible character combination until the correct password is found. While
  computationally infeasible for modern, complex passwords, it guarantees success given sufficient time and resources.

6. Password Recovery
When the guessed hash matches the target hash, the cracking software stops and displays the plaintext password, completing the
exploitation process and granting the attacker access to the protected data.



