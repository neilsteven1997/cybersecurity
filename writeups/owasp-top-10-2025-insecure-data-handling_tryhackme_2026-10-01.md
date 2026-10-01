# OWASP Top 10 2025: Insecure Data Handling

---

The OWASP Top 10 2025: Insecure Data Handling room examines three categories of application security weaknesses: A04: Cryptographic 
Failures, A05: Injection, and A08: Software or Data Integrity Failures. The exercises focus on understanding these vulnerabilities, 
their prevention, and practical exploitation through web applications.

Cryptographic failures arise when sensitive information is inadequately protected because of missing encryption, flawed implementations,
weak algorithms, exposed encryption keys, or insecure data transmission. One example is developing custom cryptographic systems instead
of relying on established and tested algorithms. Passwords should be protected with slow hashing functions such as bcrypt, scrypt, or 
Argon2. Encryption should rely on trusted libraries, while credentials and access keys should be stored in secure key management systems
or dedicated secret-storage environments rather than embedded in source code, configuration files, or repositories. The practical 
exercise uses a note-sharing application with a weak shared derivative key to demonstrate how cryptographic weaknesses can expose 
protected notes. The associated TryHackMe resource is the Cryptographic Failures Module.

Injection occurs when an application processes untrusted input in a way that allows it to become executable commands or queries. 
Vulnerable applications may pass user-controlled data directly into databases, operating system shells, or templating engines. SQL 
injection is a classic example in which login input is incorporated into a database query without proper safeguards. Other examples 
include command injection and Server-Side Template Injection (SSTI). These vulnerabilities remain relevant in the 2025 OWASP Top 10. 
Prevention requires treating all user input as untrusted, using prepared statements and parameterized queries, avoiding direct shell 
execution with user-controlled data, and applying strict validation, sanitization, and appropriate escaping. The practical exercise 
demonstrates command injection and dynamic template rendering abuse to retrieve a flag stored on the hosting machine. Recommended 
resources include the Injection Attacks Module and Command Injection.

Software or Data Integrity Failures occur when applications trust code, updates, or data without verifying authenticity, integrity, 
or origin. Examples include installing unverified software updates, loading scripts or configuration files from untrusted sources, 
accepting manipulated files, and relying on unvalidated data that affects application logic. Establishing trust boundaries and 
verifying integrity are central prevention measures. Cryptographic checks such as checksums can help validate update packages, while
access to critical artifacts should be restricted to trusted sources. Integrity controls should also extend to application build 
processes. The practical exercise demonstrates a Python deserialization attack in which malicious input is supplied to a web 
application. Related TryHackMe resources include Insecure Deserialisation and Supply Chain Attack: Lottie.

---

### Key Takeaways

- Cryptographic Failures
* Identify inadequate encryption, weak algorithms, exposed keys, and insecure transmission.
* Use bcrypt, scrypt, or Argon2 for password hashing.
* Rely on trusted cryptographic libraries instead of custom algorithms.
* Store secrets using secure key management systems.
* Understand how weak shared keys can expose protected information.

- Injection
* Treat all user input as untrusted.
* Use prepared statements and parameterized SQL queries.
* Avoid passing untrusted input directly to system shells.
* Apply strict validation, sanitization, and appropriate escaping.
* Recognize SQL injection, command injection, and Server-Side Template Injection.
- Software or Data Integrity Failures
* Establish trust boundaries for code, updates, and application data.
* Verify software and update integrity using cryptographic checks.
* Restrict modifications to critical artifacts to trusted sources.
* Apply integrity controls to build processes.
* Understand how insecure deserialization can allow malicious input to affect an application.

---
