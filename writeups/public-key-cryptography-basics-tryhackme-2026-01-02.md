# Public Key Cryptography Basics

---

In the physical world, security is often intuitive. When meeting a partner in person, authentication and authenticity are confirmed 
visually and audibly, while confidentiality is maintained by simply lowering one's voice. Translating these requirements to the 
digital realm is more complex. While symmetric encryption is efficient for protecting data confidentiality, it struggles with secure 
key distribution. Public key cryptography (asymmetric encryption) addresses this by using a mathematically linked pair of keys: a 
public key for encryption and a private key for decryption. This framework is foundational for modern authentication, integrity, and 
non-repudiation.

Asymmetric encryption is computationally expensive and significantly slower than its symmetric counterparts. Because of this, it is 
rarely used to encrypt large datasets. Instead, its primary role is to facilitate the secure exchange of symmetric keys. By using a 
server's public key to encrypt a session key, two parties can negotiate a fast symmetric cipher over an insecure channel. RSA remains 
a standard for this purpose, relying on the extreme difficulty of factoring the product of two massive prime numbers. While 
multiplying these primes is trivial, reversing the process to find the factors is practically infeasible with contemporary hardware.

The Diffie-Hellman (DH) key exchange provides an alternative method for establishing a shared secret. Unlike RSA, DH allows two 
parties to generate an identical symmetric key independently using public parameters and their own private integers. When DH is 
used alongside RSA, it creates a robust security layer: DH manages the key agreement while RSA provides the digital signatures and 
authentication necessary to prevent man-in-the-middle (MITM) attacks. I find this combination particularly effective when establishing
Secure Shell (SSH) connections, where the client must first verify the server's public key fingerprint.

Digital signatures and certificates extend these cryptographic primitives to prove identity and ensure file integrity. A digital 
signature is created by hashing a document and encrypting that hash with a private key; any recipient can then use the corresponding 
public key to verify that the file has not been tampered with. Certificates build upon this by creating a chain of trust. A root 
Certificate Authority (CA) signs the certificates of organizations, which in turn sign individual site certificates. This hierarchy 
allows browsers to verify that they are communicating with a legitimate entity, such as an official domain, rather than an imposter.

Tools like GnuPG (GPG) implement these standards for file and email encryption. GPG allows users to generate key pairs, often using 
Elliptic Curve Cryptography (ECC) or RSA, to sign and encrypt messages. In the context of security audits and Capture The Flag (CTF) 
challenges, I frequently encounter SSH private keys and GPG keys protected by passphrases. While these keys are powerful, they are 
vulnerable to brute-force or dictionary attacks if the passphrase is weak. Tools like John the Ripper can be used to crack these 
passphrases, underscoring the necessity of treating private keys with the same level of security as a primary password.

---
| Description | Code/Command |
| --- | --- |
| Generate a new SSH key pair using Ed25519 | `ssh-keygen -t ed25519` |
| View public key content | `cat ~/.ssh/id_ed25519.pub` |
| View private key content | `cat ~/.ssh/id_ed25519` |
| Specify a private key for an SSH connection | `ssh -i [private_key] [user]@[host]` |
| Generate a new GPG key pair | `gpg --full-gen-key` |
| Import a GPG key from a file | `gpg --import [keyfile]` |
| Decrypt a GPG-encrypted file | `gpg --decrypt [filename].gpg` |
| Manual page for SSH key generation | `man ssh-keygen` |

---
### Key Takeaways - Asymmetric encryption uses a public key for encryption and a private key for decryption.
* RSA security is predicated on the mathematical difficulty of factoring large prime products.
* Diffie-Hellman allows two parties to establish a shared secret over an insecure medium without pre-sharing keys.
* SSH utilizes public key fingerprints to prevent man-in-the-middle attacks during the initial handshake.
* Digital signatures prove both the origin (authenticity) and the untampered state (integrity) of a document.
* A Certificate Authority (CA) acts as a trusted third party to validate the identity of a certificate holder.
* Private keys should be stored with restricted permissions (e.g., chmod 600) and protected by strong passphrases.
* The official documentation for GPG usage can be found at the [GnuPG online manual](https://www.gnupg.org/documentation/manuals/gnupg/).
* Guidance on trusted CAs is maintained by browser vendors such as [Mozilla](https://wiki.mozilla.org/CA) and [Google](https://pki.goog/repository/).
* Get your own TLS certificates for domains you own using [Let's Encrypt](https://letsencrypt.org/) for free.
---




