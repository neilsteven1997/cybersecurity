# Hashing Basics

---

In my recent lab sessions, I have been refining my understanding of cryptographic hash functions and their practical applications 
in verifying data integrity and securing authentication. A hash function operates as a one-way mathematical transform, mapping input 
data of any size to a fixed-length string known as a digest or hash value. Unlike encryption, hashing is designed to be computationally 
irreversible; there is no key to decrypt a hash. A critical property of a robust hashing algorithm is the avalanche effect, where a 
single bit change in the input—such as the one-bit difference between the ASCII characters "T" (0x54) and "U" (0x55)—results in a 
completely different output. Tools like `md5sum`, `sha1sum`, and `sha256sum` are essential for these calculations, typically outputting 
results in hexadecimal format where each byte is represented by two hex digits.

I frequently use hashing to verify file integrity, especially after large downloads or when receiving data from third parties. By 
comparing the hash of a local file against an official checksum, such as those provided by the Fedora Project, I can confirm the file 
is bit-for-bit identical to the original and has not been corrupted or tampered with. Beyond integrity, hashing is the industry 
standard for password storage. Secure practices dictate that servers should never store plaintext passwords. Instead, they store a 
salted hash. Salting involves prepending or appending a unique, random string to the password before hashing, which effectively 
prevents the use of precomputed rainbow tables—large lookup tables that trade disk space for time to crack unsalted hashes. Modern 
algorithms like Argon2, Bcrypt, and Scrypt are specifically designed with high computational costs to resist high-speed cracking 
attempts using GPUs.

From an offensive research perspective, identifying and cracking hashes requires a blend of automated tools and contextual analysis. 
On Linux systems, password hashes are located in `/etc/shadow`, prefixed by identifiers that indicate the algorithm, such as `$y$` 
for Yescrypt or `$6$` for SHA-512. Windows environments utilize NTLM hashes stored in the Security Accounts Manager (SAM). When 
automated recognition tools like `hashID` prove unreliable, I rely on the length and encoding of the string to determine the type. 
Cracking is not a decryption process but a high-speed guessing game where tools like Hashcat or John the Ripper hash wordlists 
(e.g., the 14-million-entry `rockyou.txt`) and compare the results to the target hash. I have found it most efficient to run Hashcat 
on host hardware to leverage the parallel processing power of the GPU, as virtual machines often lack direct hardware access.

For scenarios requiring both integrity and authenticity, the Keyed-Hash Message Authentication Code (HMAC) is indispensable. It 
combines a cryptographic hash with a secret key, ensuring that the message has not been altered and that the sender is legitimate. 
This is distinct from simple encoding, like Base64, which merely transforms data for compatibility and offers no security or 
confidentiality. As I document these findings, the distinction remains clear: encoding is for compatibility, encryption is for 
confidentiality, and hashing is for integrity and secure non-reversible storage. Further research into specialized tools can be 
found at John the Ripper's project page or through general cryptography deep dives.

The construction of `Hash-based Message Authentication Codes (HMAC)` represents a critical fusion of hashing algorithms and secret 
keys to validate both the source and the content of a transmission. In my experience, relying solely on standard digests is 
insufficient for security; the HMAC structure introduces specific padding—namely ipad and opad—to mitigate vulnerabilities like 
length extension attacks. The logic is fundamentally nested, requiring an initial pass where the key is modified by the inner 
padding and combined with the message before a second hashing pass incorporates the outer padding to finalize the digest.

This layered approach ensures that the integrity of the message is intrinsically linked to the possession of the secret key, making 
it computationally impractical for an adversary to generate a valid authentication tag. I particularly value how this design 
abstracts the underlying hash function, allowing for flexible implementation with various algorithms while maintaining the same 
rigorous authentication logic.

`HMAC(K,M) = H((K⊕opad)||H((K⊕ipad)||M))`

- K: The secret key utilized for authentication.
- M: The specific message payload requiring verification.
- opad: The outer padding block used in the final hash computation.
- ipad: The inner padding block used in the initial hash stage.
- The authentication process follows a two-step nested hash: an inner keyed-message computation followed by an outer keyed-result 
refinement.
- This methodology ensures strong protection against forgery, as valid digests cannot be generated without access to the secret key.

---
| Description | Code/Command |
| --- | --- |
| View file content | `cat file1.txt` |
| View file in hexadecimal and ASCII | `hexdump -C file1.txt` |
| Generate MD5 hash for all text files | `md5sum *.txt` |
| Generate SHA1 hash for all text files | `sha1sum *.txt` |
| Generate SHA256 hash for all text files | `sha256sum *.txt` |
| Count lines in a wordlist file | `wc -l rockyou.txt` |
| View the first ten lines of a file | `head rockyou.txt` |
| View the Linux shadow password file | `sudo cat /etc/shadow` |
| Basic Hashcat straight attack (Bcrypt) | `hashcat -m 3200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt` |
| Encode a string to Base64 | `echo "TryHackMe" |
| Decode a Base64 string | `echo "VHJ5SGFja01lCg==" |

---
### Key Takeaways

* Hashing is a one-way function; it is mathematically designed to be irreversible and does not utilize a key like encryption.
* Any change to the input data, no matter how minor, must result in a significantly different hash value to prevent collisions.
* Passwords must never be stored in plaintext or with reversible encryption; use secure, salted hashing algorithms like Argon2, Bcrypt, or Scrypt.
* Salts must be unique per user to neutralize rainbow table attacks and ensure that identical passwords result in different hashes.
* Use `sha256sum` or similar utilities to verify that downloaded software matches the developer's original bitstream.
* GPU-based cracking is significantly faster than CPU cracking for many algorithms, though some modern hashes are "memory-hard" to resist this acceleration.
* On Linux, the `/etc/shadow` file format (`$prefix$options$salt$hash`) provides the necessary metadata to identify the hashing algorithm in use.
* Encoding (Base64, ASCII) is not a security measure and can be easily reversed by anyone without a key.

---

