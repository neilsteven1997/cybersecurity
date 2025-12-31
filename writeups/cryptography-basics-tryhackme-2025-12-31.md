# Cryptography Basics

---
Cryptography serves as the fundamental mechanism for ensuring data confidentiality, integrity, and authenticity in the presence of 
adversarial third parties. Modern applications of these techniques are ubiquitous, protecting everything from web authentication and SSH 
tunnels to financial transactions and software downloads. Beyond technical implementation, cryptography is a regulatory necessity. 
Frameworks such as PCI DSS mandate encryption for payment data both at rest and in motion, while healthcare regulations like HIPAA in the 
US and GDPR in the EU require robust data protection to ensure the privacy of medical records.

The core process of cryptography involves transforming readable data, known as plaintext, into an unreadable format called ciphertext 
through an encryption function. This transformation requires a cipher—the underlying mathematical algorithm—and a specific key. To revert 
ciphertext to plaintext, a decryption function is applied using the corresponding key. While the security of modern systems relies on 
keeping the key secret, the ciphers themselves are generally public knowledge. Historically, simpler methods like the Caesar Cipher relied
on basic character shifts, but these are now considered insecure due to the limited number of possible keys, which allows for trivial 
brute-force recovery.

Encryption is categorized into two primary types: symmetric and asymmetric. Symmetric encryption utilizes a single private key for 
both encryption and decryption, necessitating a secure out-of-band channel for key distribution. Common standards include the Advanced 
Encryption Standard (AES), which replaced the now-obsolete Data Encryption Standard (DES). Conversely, asymmetric or public-key encryption 
employs a mathematically linked key pair: a public key for encryption and a private key for decryption. While asymmetric methods like RSA 
and Elliptic Curve Cryptography (ECC) solve the key distribution problem, they are computationally more intensive and typically require 
larger key sizes to achieve security levels comparable to symmetric ciphers.

The mathematical foundation of these algorithms often rests on operations like XOR (exclusive OR) and modulo arithmetic. XOR is a bitwise 
operation where identical bits result in zero and differing bits result in one. Its associative and self-inverting properties make it a 
building block for symmetric ciphers. Modulo arithmetic, which calculates the remainder of a division, is equally critical, particularly 
in asymmetric algorithms. The non-reversible nature of the modulo operation—where a remainder does not uniquely identify the original 
dividend—provides the "one-way" functionality necessary for secure cryptographic primitives.

---
| Description | Code/Command |
| --- | --- |
| XOR Truth Table | 0⊕0=0, 0⊕1=1, 1⊕0=1, 1⊕1=0 |
| XOR Encryption Property | C = P ⊕ K |
| XOR Decryption Property | P = C ⊕ K |
| Modulo Operation (Python) | `x % y` |
| Caesar Cipher Shift (Example) | `(char_index + shift) % 26` |

---
### Key Takeaways* Confidentiality ensures data is unreadable to unauthorized parties; Integrity ensures it has not been altered.
* Symmetric encryption is fast but faces challenges in secure key distribution.
* Asymmetric encryption enables secure communication over public channels using public/private key pairs.
* AES is the current industry standard for symmetric encryption, with key lengths of 128, 192, or 256 bits.
* RSA security relies on the difficulty of factoring large integers, while ECC offers similar security with significantly shorter keys.
* The XOR operation is a primary component in many ciphers because applying the same key twice reverts the data to its original state.

---
