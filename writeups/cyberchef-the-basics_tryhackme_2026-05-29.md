# CyberChef: The Basics

---

CyberChef serves as a versatile browser-based application for executing a wide range of data transformation tasks relevant to 
cybersecurity work. It functions as a comprehensive toolkit where users chain together individual operations into ordered sequences 
called recipes to process input data efficiently. The primary goals center on understanding the tool itself, navigating its layout, 
recognizing frequently used operations, and building effective recipes.

Access options include the hosted online version reachable through a standard web browser with internet connectivity. For offline 
capability, the latest stable release can be downloaded from the official GitHub repository and run locally on Windows or other 
platforms. Familiarity with foundational topics such as hashing and cryptography proves helpful though not strictly required.

The interface organizes into four distinct panes: Operations, Recipe, Input, and Output. The Operations pane maintains an extensive 
categorized library of functions, searchable for quick location. Hovering over entries reveals descriptions, usage examples, and 
external references such as Wikipedia links. Common operations encountered include From Morse Code for translating code sequences into 
alphanumeric text, URL Encode for converting special characters into percent-encoded format suitable for URIs, To Base64 for producing
ASCII Base64 strings, To Hex for byte representation with delimiters, To Decimal for ordinal integer arrays, and ROT13 as a basic 
Caesar shift substitution.

The Recipe pane acts as the central workspace where selected operations are dragged, ordered, and configured with specific arguments.
Controls allow saving current recipes for reuse, loading previously stored ones, clearing the current sequence, and executing the full
chain via a Bake button. An Auto Bake option enables real-time processing as changes occur. The Input pane accepts data through 
direct typing, pasting, file upload, or even entire folder imports, with support for multiple tabs to handle separate datasets. The 
Output pane displays transformation results and includes options to save as .dat files, copy raw content to clipboard, swap output 
back into input, or reset the overall layout.

Effective usage follows a structured four-step mental process. First, establish a precise objective such as determining the meaning 
behind an encoded string found during analysis. Second, load the raw data into the input area. Third, identify and configure relevant
operations, often starting within categories like Encryption/Encoding when dealing with obfuscated content. Fourth, examine the output
to verify achievement of the initial goal, iterating with adjustments if necessary.

Several operation categories stand out for routine tasks. Under Extractors, functions pull IPv4 and IPv6 addresses, URLs (requiring 
protocol specification to reduce false positives), and email addresses matching standard patterns. Date and Time operations handle 
conversions between UNIX timestamps and readable datetime strings, where a UNIX timestamp counts seconds since the 1970 epoch. Data 
Format operations cover decoding from Base64, Base85, and Base58, URL decoding of percent-encoded strings, and related encoding 
variants like Base62. Base encodings generally convert binary data into text representations using defined ASCII character sets.

A manual walkthrough of Base64 encoding for the string "THM" illustrates the underlying mechanics. Using ASCII values converted to 
binary (T: 01010100, H: 01001000, M: 01001101), concatenate to 24 bits, regroup into 6-bit segments, convert each to decimal, then 
map against the Base64 index table to yield VEhN. URL Decode reverses percent-encoding back to original characters based on UTF-8 
standards, commonly handling sequences such as %3A for colon or %2F for slash.

Practical application involves downloading task files, loading their content into the input pane, and applying appropriate extractors
or format operations to derive results. The tool excels at intuitive data manipulation for Base64, binary handling, URL processing, 
and extraction of indicators like IPs, emails, and domains, though very large datasets may require supplementary tooling for optimal
performance.

---

**Code/Command Table**

| Description | Code/Command |
|-------------|-------------|
| Morse Code example conversion | - .... .-. . .- - ... |
| URL Encode example | https://tryhackme.com/r/room/cyberchefbasics |
| To Base64 example | This is fun! |
| To Hex example | This Hex conversion is awesome! |
| ROT13 example | Digital Forensics and Incident Response |
| From Base64 example | V2VsY29tZSB0byB0cnloYWNrbWUh |
| Base64 manual result for THM | VEhN |

---

**Extracted Tables**

**Operations Examples**

| Operations | Description | Examples |
|------------|-------------|----------|
| From Morse Code | Translates Morse Code into (upper case) alphanumeric characters. | - .... .-. . .- - ... becomes THREATS when used with default parameters |
| URL Encode | Encodes problematic characters into percent-encoding, a format supported by URIs/URLs. | https://tryhackme.com/r/room/cyberchefbasics becomes https%3A%2F%2Ftryhackme%2Ecom%2Fr%2Froom%2Fcyberchefbasics when used with the parameter “Encode all special chars” |
| To Base64 | This operation encodes raw data into an ASCII Base64 string. | This is fun! becomes VGhpcyBpcyBmdW4h |
| To Hex | Converts the input string to hexadecimal bytes separated by the specified delimiter. | This Hex conversion is awesome! becomes 54 68 69 73 20 48 65 78 20 63 6f 6e 76 65 72 73 69 6f 6e 20 69 73 20 61 77 65 73 6f 6d 65 21 |
| To Decimal | Converts the input data to an ordinal integer array. | This Decimal conversion is awesome! becomes 84 104 105 115 32 68 101 99 105 109 97 108 32 99 111 110 118 101 114 115 105 111 110 32 105 115 32 97 119 101 115 111 109 101 33 |
| ROT13 | A simple Caesar substitution cipher which rotates alphabet characters by the specified amount (default 13). | Digital Forensics and Incident Response becomes Qvtvgny Sberafvpf naq Vapvqrag Erfcbafr |

---

**Extractors Category**

| Specific | Description |
|----------|-------------|
| Extract IP addresses | Extracts all IPv4 and IPv6 addresses. |
| Extract URLs | Extracts Uniform Resource Locators (URLs) from the input. The protocol (http, https, etc.) is required, otherwise there will be far too many false positives. |
| Extract email addresses | Extracts all email addresses from the input. |

---

**Date / Time Category**

| Specific | Description |
|----------|-------------|
| From UNIX Timestamp | Converts a UNIX timestamp to a datetime string. |
| To UNIX Timestamp | Parses a datetime string in UTC and returns the corresponding UNIX timestamp. |

---

**Data Format Category**

| Operations | Description | Examples |
|------------|-------------|----------|
| From Base64 | This operation decodes data from an ASCII Base64 string back into its raw format. | V2VsY29tZSB0byB0cnloYWNrbWUh becomes Welcome to tryhackme! |
| URL Decode | Converts URI/URL percent-encoded characters back to their raw values. | https%3A%2F%2Fgchq%2Egithub%2Eio%2FCyberChef%2F becomes https://gchq.github.io/CyberChef/ |
| From Base85 | Notation for encoding arbitrary byte data. It is usually more efficient than Base64. This operation decodes data from an ASCII string (with an alphabet of your choosing, presets included). | BOu!rD]j7BEbo7 becomes hello world |
| From Base58 | Notation for encoding arbitrary byte data. It differs from Base64 by removing efficiently misread characters (i.e. l, I, 0, and O) to improve human readability. | AXLU7qR becomes Thm58 |
| To Base62 | Notation for encoding arbitrary byte data using a restricted set of symbols that humans can conveniently use and process by computers. The high number base results in shorter strings than with the decimal or hexadecimal system. | Thm62 becomes 6NiRkOY |

---

**ASCII Table (Partial)**

| Decimal | Binary | Symbol | Decimal | Binary | Symbol |
|---------|--------|--------|---------|--------|--------|
| 65 | 01000001 | A | 78 | 01001110 | N |
| 66 | 01000010 | B | 79 | 01001111 | O |
| 67 | 01000011 | C | 80 | 01010000 | P |
| 68 | 01000100 | D | 81 | 01010001 | Q |
| 69 | 01000101 | E | 82 | 01010010 | R |
| 70 | 01000110 | F | 83 | 01010011 | S |
| 71 | 01000111 | G | 84 | 01010100 | T |
| 72 | 01001000 | H | 85 | 01010101 | U |
| 73 | 01001001 | I | 86 | 01010110 | V |
| 74 | 01001010 | J | 87 | 01010111 | W |
| 75 | 01001011 | K | 88 | 01011000 | X |
| 76 | 01001100 | L | 89 | 01011001 | Y |
| 77 | 01001101 | M | 90 | 01011010 | Z |

---

**Base64 Index Table**

| Index | Character | Index | Character | Index | Character |
|-------|-----------|-------|-----------|-------|-----------|
| 0 | A | 26 | a | 52 | 0 |
| 1 | B | 27 | b | 53 | 1 |
| ... | ... | ... | ... | ... | ... |
| 21 | V | 4 | E | 33 | h |
| 13 | N | - | - | - | - |

---

### Key Takeaways
- Define a clear, achievable objective before starting any processing.
- Load raw data into the Input pane via paste, type, or file upload.
- Select and configure operations from relevant categories such as Extractors, Date/Time, or Data Format.
- Execute the recipe and evaluate whether the Output meets the original objective, iterating as needed.
- Use Auto Bake for immediate feedback during recipe development.
- Save useful recipes for future reuse across similar tasks.
- For Base64 encoding, convert text to binary, regroup into 6-bit segments, map to decimal, then apply the Base64 character index.
- Apply Extract IP addresses, Extract URLs, and Extract email addresses when pulling indicators from unstructured data.

---





