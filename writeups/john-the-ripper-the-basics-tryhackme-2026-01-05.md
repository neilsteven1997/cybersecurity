# John the Ripper: The Basics

---
Hashing functions operate as the bedrock of modern cryptographic storage, transforming arbitrary data into fixed-length digests 
through one-way mathematical processes. The security of these functions rests on computational intractability, often framed through 
the P vs NP problem; while calculating a hash is efficient (Polynomial time), reversing it to find the original input is currently 
intractable (Non-deterministic Polynomial time). In practical scenarios, such as auditing a system or conducting a penetration test, 
the most effective way to bypass this "irreversibility" is through dictionary attacks. By hashing an extensive list of potential 
passwords and comparing the results to a target hash, tools like John the Ripper (JTR) can reveal the original plaintext. I prefer 
using the "Jumbo John" version, as it includes essential community-contributed tools that the core distribution lacks. Setting up the 
environment correctly is half the battle, usually requiring the RockYou wordlist—a 2009 breach artifact often found 
at `/usr/share/wordlists` on Kali or in the SecLists repository at [https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists).

Efficiency in cracking is highly dependent on identifying the correct hash format. While JTR has an auto-detection feature, it is 
prone to misidentification. I frequently rely on `hash-identifier` to determine the likely algorithm before manual execution; the 
Python script can be sourced directly from GitLab at [https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py](https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py). 
When a specific format is identified, such as MD5, JTR usually requires the `raw-` prefix in the `--format` flag to distinguish 
standard hashes from more complex application-specific formats. This precision is vital when moving into operating system authentication. 
Windows, for instance, utilizes NTLM (or NThash) within the Security Account Manager (SAM) database. On Linux, password hashes 
reside in `/etc/shadow`, but JTR cannot parse this file in isolation. It requires the `unshadow` utility to merge `/etc/shadow` 
with `/etc/passwd`, providing the necessary user context for the cracking engine.

Advanced cracking strategies often move beyond simple wordlists into heuristic modes. Single Crack mode is particularly potent 
for targeting users who employ predictable patterns based on their own identity. By mangling the username or information found 
in the UNIX GECOS field—which stores details like full names or office numbers—JTR can generate a highly relevant, localized 
wordlist. If these automated mangling rules fail, I define custom rules within the `john.conf` configuration file (typically 
located in `/etc/john/` or `/opt/john/`). These rules allow for granular control over word mutations, such as prepending capitals 
or appending specific numeric and symbol patterns to satisfy complexity requirements. This ability to exploit human predictability 
in password selection is what often makes an otherwise robust algorithm vulnerable.

Beyond operating system credentials, the utility of the JTR suite extends to encrypted archives and private keys. The workflow 
remains consistent across different file types: extract the hash into a JTR-readable format using a specific "2john" utility, 
then feed that hash into the cracker. For password-protected ZIP and RAR files, `zip2john` and `rar2john` are the standard tools. 
Similarly, when encountering SSH private keys (id_rsa) that are passphrase-protected, the `ssh2john.py` script—often found 
in `/opt/john/`—converts the key into a format suitable for dictionary attacks. For those operating on Windows, pre-compiled 
binaries for 64-bit ([https://www.openwall.com/john/j/john-1.9.0-jumbo-1-win64.7z](https://www.google.com/search?q=https://www.openwall.com/john/j/john-1.9.0-jumbo-1-win64.7z)) 
and 32-bit ([https://www.openwall.com/john/j/john-1.9.0-jumbo-1-win-x86.7z](https://www.google.com/search?q=https://www.openwall.com/john/j/john-1.9.0-jumbo-1-win-x86.7z)) 
systems facilitate these same operations outside of a Linux environment.

---
| Description | Code/Command |
| --- | --- |
| Basic John syntax | `john [options] [file_path]` |
| Cracking using a specific wordlist | `john --wordlist=[path_to_wordlist] [path_to_file]` |
| Fetching hash-identifier | `wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py` |
| Launching hash-identifier | `python3 hash-id.py` |
| Manual format specification | `john --format=[format] --wordlist=[path_to_wordlist] [path_to_file]` |
| List all supported formats | `john --list=formats` |
| Combine passwd and shadow files | `unshadow [path_to_passwd] [path_to_shadow] > unshadowed.txt` |
| Execute Single Crack mode | `john --single --format=[format] [path_to_file]` |
| Execute custom rule | `john --wordlist=[path] --rule=[RuleName] [path_to_file]` |
| Extract hash from ZIP | `zip2john [zip_file] > [output_file]` |
| Extract hash from RAR | `rar2john [rar_file] > [output_file]` |
| Extract hash from SSH key | `python3 ssh2john.py [id_rsa_file] > [output_file]` |

---
### Successful Exploitation
After mounting the host filesystem from the compromised container, the target DoorDash website was defaced:
<p align="center">
  <table>
    <tr>
      <td><img src="images/day-14-aoc-2025-defaced-website.png" alt="DoorDash website defaced with Hopperoo message after container escape" 
  width="450"/>
      <td><img src="images/day-14-aoc-2025-restored-website.png" alt="Restored website" width="450"/></td>
    </tr>
    <tr>
      <td align="center"><strong>Figure 1a:</strong> Final defacement after container escape</td>
      <td align="center"><strong>Figure 1b:</strong> Restored website after running restoration script</td>
    </tr>
    <tr>
      <td><img src="images/day-14-aoc-2025-deployer-bash-flag.png" alt="Using deployer bash to find the flag" 
  width="450"/>
      <td><img src="images/day-14-aoc-2025-secret-code.png" alt="Finding secret code by incrementing the number on website link" width="450"/></td>
    </tr>
     <tr>
      <td align="center"><strong>Figure 2a:</strong> Using deployer bash to find the flag</td>
      <td align="center"><strong>Figure 2b:</strong> Incrementing the number on link to find secret code</td>
    </tr>
  </table>
</p>
---
### Key Takeaways

* Hashes are one-way transformations; while calculating them is mathematically "easy," reversing them is computationally "hard"
  (NP-intractable).
* Dictionary attacks succeed by hashing a known list (wordlist) and looking for matches against the target.
* Use the Jumbo version of John the Ripper to ensure access to essential utilities like `unshadow` and the various `2john` converters.
* Identify the hash type manually before cracking to increase reliability and speed.

* To crack Linux passwords:
* Obtain a copy of the target `/etc/passwd` and `/etc/shadow` files.
* Run `unshadow` to merge them into a single file.
* Provide the unshadowed output to John.

* Single Crack mode leverages "word mangling" to create passwords based on the username and GECOS field information.
* Custom rules in `john.conf` allow you to define specific patterns, such as capitalizations (`c`), prepending (`A0`), and
  appending (`Az`).

* To crack protected files (Zip, RAR, SSH):
* Run the appropriate conversion tool (`zip2john`, `rar2john`, or `ssh2john.py`).
* Redirect the output to a text file.
* Run John against the resulting text file using a wordlist.

* Rar: `unrar x -ppasswordhere secure.ra`
* Zip: `unzip -P pass123 secure.zip`

---
