# OWASP Top 10 2025: IAAA Failures

---

This entry covers the TryHackMe room focused on three categories from the OWASP Top 10 2025 that stem from failures in Identity, 
Authentication, Authorisation, and Accountability (IAAA) implementation within web applications. The material targets beginners and 
pairs theoretical coverage of A01 Broken Access Control, A07 Authentication Failures, and A09 Logging and Alerting Failures with 
practical static-site challenges. Official details on the broader ranking appear at 
https://owasp.org/Top10/2025/0x00_2025-Introduction/, while the room itself is available at 
https://tryhackme.com/room/owasptopten2025one.

IAAA provides a sequential model for verifying users and their actions. Identity establishes the unique account representing a person 
or service, typically via user ID or email. Authentication then proves that identity through mechanisms such as passwords, one-time 
passwords, or passkeys. Authorisation determines the actions permitted to that verified identity. Accountability records and alerts on
the who, what, when, and where of those actions. Each layer depends on the prior one; skipping any leaves later controls ineffective.
Weaknesses across these layers enable threat actors to reach other users’ data or obtain elevated privileges beyond their intended 
scope. A dedicated deeper treatment of the model is recommended before advancing.

Broken Access Control arises when the server fails to enforce access decisions on every request. A frequent manifestation is Insecure 
Direct Object Reference, in which altering an identifier in a parameter such as an accountID value exposes or permits modification of 
another user’s data. This produces either horizontal privilege escalation, allowing access to peer resources within the same role, or
vertical privilege escalation that reaches administrative functions, because the application places excessive trust in client-supplied 
input. The accompanying static site lets the accountID parameter be varied until the account holding more than one million dollars is 
located. Further exploration of encoded or hashed variants appears in the linked Broken Access Control and Insecure Direct Object 
References rooms.

Authentication Failures occur when an application cannot reliably verify or bind an identity. Typical weaknesses include username 
enumeration, weak or guessable passwords without lockout or rate limiting, logic flaws in registration or login flows, and insecure
session or cookie handling. Any of these can let an attacker authenticate as another user or attach a session to the wrong account. 
In the practical exercise the username admin is known; registering an account under the variant aDmiN and then authenticating grants 
access to the administrative dashboard. Expanded coverage of related techniques such as brute force, session handling, cookies, JWT, 
and OAuth is available in the Authentication Bypass, Multi-Factor Authentication, and Authentication Module rooms.

Logging and Alerting Failures leave defenders unable to detect or investigate attacks because security-relevant events are neither 
recorded nor signalled. Accountability rests on complete records of who performed which action, when, and from where. Concrete 
shortcomings include absent authentication events, vague error messages, missing alerts for brute-force attempts or privilege changes,
short retention periods, and storage locations that attackers can alter. The static site supplies logs of an ongoing attack; examining
them reveals the sequence of events and underscores how incomplete data would hinder reconstruction. A dedicated logging-focused room
provides additional depth on accountability practices.

The exercises reinforce that proper IAAA implementation underpins the three examined categories and that gaps in any layer produce 
the listed OWASP risks. Continued study proceeds to the subsequent room on Application Design Flaws.

---

### Key Takeaways
- Enforce server-side checks on every request to prevent Broken Access Control.
- Enforce unique indexes on the canonical form of identifiers, rate-limit or lock out brute-force attempts, and rotate sessions on
  password or privilege changes to address Authentication Failures.
- Log the full authentication lifecycle including failures, successes, password or multi-factor changes, role changes and
  administrative actions; centralise logs off-host with adequate retention; and alert on anomalies such as brute-force bursts or
  privilege elevation to mitigate Logging and Alerting Failures.

---




