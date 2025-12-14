# YARA Rules

---
## YARA Overview and Applications**

YARA is a foundational tool in threat intelligence and incident response, designed to identify and categorize malware by 
detecting unique, characteristic patterns within files, code, and memory. It functions as a digital fingerprinting mechanism, 
allowing security analysts to scan large volumes of data for subtle traces of malicious activity that might otherwise evade 
detection. Within an enterprise security operations context, YARA serves as a critical guardian, ensuring that faint indicators 
of compromise (IOCs) are not overlooked. 

YARA's primary value lies in its power to detect malware based on underlying behavior and defined patterns rather than relying 
solely on file names or static signatures. This flexibility allows defenders to define custom rules based on specific threat 
intelligence, enabling proactive threat hunting and rapid response to new or evolving malware variants.

### Defenders rely on YARA in several key scenarios:

* **Post-incident analysis:** Validating the complete eradication of malware traces across an environment following a compromise.
* **Threat Hunting:** Actively searching endpoints and systems for indicators of known or related malware families.
* **Intelligence-based scans:** Applying external or shared YARA rules to proactively detect new IOCs identified by other organizations.
* **Memory analysis:** Examining memory dumps for active, transient malicious code fragments that do not persist on disk.

YARA provides distinct operational advantages, offering **speed** for scanning large data sets, **flexibility** for matching 
complex patterns, **control** over defining maliciousness criteria, and **shareability** for collaborative defense. Ultimately, 
it empowers defenders to transition from passive monitoring to active, intelligence-driven hunting.

## YARA Rule Structure

A YARA rule is constructed from three fundamental components:

* **Metadata:** Descriptive information such as the author, creation date, purpose, and confidence level, which is highly
  recommended for maintaining large rule sets.
* **Strings:** The actual clues YARA searches for, encompassing text, byte sequences (hexadecimal), or complex patterns
  (regular expressions).
* **Conditions:** The logical expression that determines when the rule successfully triggers, based on combinations of string
  matches and file properties.

## String Types and Modifiers

YARA uses three main types of strings to define search signatures:

1.  **Text Strings:** The simplest, most common type. By default, they are case-sensitive ASCII, but their capability can be
   significantly extended using modifiers:
2.  **Hexadecimal Strings:** Used to search for specific raw byte patterns, essential for detecting file headers, shellcode, or
   binary fragments that lack readable text. These can include wildcards (`??`) for variable bytes.
3.  **Regular Expression Strings:** Provide flexible search patterns for matching multiple variations of a malicious string,
   useful for volatile data like URLs, encoded commands, or mutating file names. Caution is required, as overly broad regex can
   increase scan time and false positives.

| Description | Code/Command |
| :--- | :--- |
| Basic YARA rule structure | `rule TBFC_KingMalhare_Trace { meta: strings: condition: }` |
| Text string definition | `$TBFC_string = "Christmas"` |
| Case-insensitive modifier | `nocase` |
| Wide-character modifier | `wide` |
| ASCII enforcement modifier | `ascii` |
| Single-byte XOR decryption search | `xor` |
| Base64 string decoding search | `base64` |
| Hexadecimal string with wildcard | `{ 4D 5A 90 00 }` |
| Regex for URL pattern | `/http:\/\/.*malhare.*/ nocase` |
| YARA command for recursive scan | `yara -r icedid_starter.yar C:\` |
| YARA flag to print matching strings | `-s` |

## Rule Condition Logic

The `condition` section is the decision-making core of a YARA rule, combining string match results and file properties using 
logical operators.

* **Match a single string:** The rule triggers if one defined string variable is found (e.g., `condition: $xmas`).
* **Match any string:** Triggers if at least one of the defined strings is present (`condition: any of them`).
* **Match all strings:** Requires that every single defined string is found to trigger the rule (`condition: all of them`).
* **Combined logic:** Uses logical operators (`and`, `or`, `not`) to create complex detection strategies (e.g., `($s1 or $s2)
  and not $benign`).
* **File property comparisons:** Includes checks against file metadata, such as file size (`filesize < 700KB`) or entry point.

**YARA Study Use Case: IcedID Detection**

To detect a trojan like IcedID, the rule combines specific strings with a size constraint. For instance, combining the **MZ 
header** (`{ 4D 5A }`), a malicious binary fragment, and a story-based IOC string (`"malhare" nocase`) with the logical 
requirement that **all** must be present, and the file size must be less than 10MB (`filesize < 10485760`), forms a 
high-fidelity detection rule. This rule is then executed recursively across a target drive using the command `yara -r 
icedid_starter.yar C:\`.

---

### Key Takeaways

* YARA rules are composed of three parts: **Metadata**, **Strings** (text, hex, regex), and **Conditions** (logic).
* **Strings** can be modified using keywords like `nocase`, `wide`, and `xor` to evade common malware obfuscation techniques.
* **Conditions** use logical operators (`and`, `or`, `not`) and can incorporate file properties like `filesize` for precise detection.
* The command line flag `-r` allows YARA to scan directories recursively, while `-s` prints the specific strings that caused a match.
* A practical YARA command to search for the keyword "TBFC:" followed by an ASCII alphanumeric keyword across a specific
  directory: `rule TBFC_Search { strings: $msg = /TBFC:\s*[A-Za-z0-9]+/ condition: $msg }` (or similar regex tailored to the
  exact alphanumeric set).



