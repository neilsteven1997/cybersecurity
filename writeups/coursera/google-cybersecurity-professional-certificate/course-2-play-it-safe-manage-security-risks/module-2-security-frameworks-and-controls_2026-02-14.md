# Module 2: Security Frameworks And Controls

---

In my work as a cybersecurity researcher, I've been reflecting on the foundational elements that define how organizations shield themselves from data breaches and maintain operational resilience. The role of a security analyst stands out as pivotal, focusing on defending not just corporate assets but also the personal and financial well-being of individuals whose information could be exposed. Daily routines in this field directly contribute to broader safety, drawing from structured approaches like security frameworks and controls to audit and fortify systems. These frameworks serve as blueprints for crafting tailored policies that address overlapping risks, extending protection to both digital and physical realms—think access controls like key cards for buildings alongside defenses against phishing or other social engineering tactics. I find it striking how the human factor often emerges as the primary vulnerability, underscoring the need for awareness training and rapid reporting channels to curb potential incidents.

Delving deeper, security controls emerge as targeted measures to avert losses, whether financial or reputational, by countering threats such as unauthorized entry or fraudulent activities. Encryption, for instance, converts sensitive data into unreadable ciphertext to preserve confidentiality, while authentication methods—ranging from basic passwords to advanced multi-factor setups incorporating biometrics—confirm user identities. Authorization then layers on by restricting access to only what's essential, aligning closely with the CIA triad model that balances confidentiality (limiting data to those with a need-to-know), integrity (ensuring data remains accurate and unaltered), and availability (guaranteeing timely access for authorized users). In practice, this triad guides risk management, as seen in banking where personal details are guarded fiercely, accounts are frozen amid suspicious patterns to verify ownership, and online portals are kept operational with robust validation.

Organizations leverage specific frameworks like the NIST Risk Management Framework and Cybersecurity Framework to integrate these controls effectively, aiding compliance with regulations such as HIPAA for healthcare data protection. The Cyber Threat Framework from the ODNI provides a standardized language for describing threat activities, enhancing efficiency in analysis and response across evolving landscapes. Similarly, ISO/IEC 27001 outlines requirements for information security management systems, offering best practices for handling assets like employee data or intellectual property; its official details are at https://www.iso.org/standard/27001. Controls fall into physical (e.g., fences, guards, CCTV), technical (e.g., firewalls, antivirus, MFA), and administrative categories (e.g., separation of duties, asset classification). For health-related protections, the U.S. Department of Health and Human Services' Physical Access Control presentation offers insights, available at https://www.hhs.gov/sites/default/files/physical-access-control.pdf.

The CIA triad remains a cornerstone for analysts, informing system setups and policies to uphold a strong security posture amid changes. Confidentiality is bolstered by principles like least privilege, restricting access to job necessities. Integrity relies on tools like cryptography and encryption to prevent tampering, while availability ensures remote workers can connect securely without overexposing unrelated data. NIST's Cybersecurity Framework, detailed at https://www.nist.gov/cyberframework, voluntary yet powerful, features five core functions: Identify (assessing risks to people and assets), Protect (deploying policies and tools based on historical insights), Detect (enhancing monitoring for swift incident spotting), Respond (containing and analyzing breaches with process improvements), and Recover (restoring normal operations post-event). NIST SP 800-53, focused on federal systems, unifies controls for the CIA triad; its full document is at https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53r5.pdf.

OWASP principles complement these, promoting secure design to minimize vulnerabilities. Minimizing attack surfaces involves disabling unused features and enforcing strong passwords, while least privilege and defense in depth—layering controls like MFA and intrusion detection—limit breach impacts. Separation of duties prevents misuse by distributing privileges, and keeping security simple avoids complexity that could impede teams. When issues arise, fixing them correctly means rooting out causes, containing effects, and testing remediations. Additional principles include establishing secure defaults (making apps safe out-of-the-box), failing securely (defaulting to secure states on failure), not trusting external services (verifying third-party security), and avoiding security by obscurity (relying on robust factors beyond hidden details). For mobile contexts, the OWASP Mobile Top 10 is referenced at https://owasp.org/www-project-mobile-top-10.

Security audits tie it all together, reviewing controls, policies, and procedures against internal standards and external regulations like GDPR (official site at https://gdpr-info.eu), PCI DSS (standards at https://www.pcisecuritystandards.org/standards/pci-dss), or HIPAA (guidelines at https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html). Internal audits, led by compliance officers, define scopes covering assets and technologies, set goals for security objectives, assess risks to prioritize measures, evaluate control effectiveness across categories, check regulatory adherence, and communicate findings with mitigation recommendations. Factors influencing audits include industry, size, and location, with frameworks like NIST CSF or ISO 27000 streamlining the process. Checklists typically cover scope identification, risk assessments, audit execution, mitigation planning, and stakeholder reporting. For further depth, NIST's assessment and auditing resources are useful at https://www.nist.gov/cyberframework/assessment-auditing-resources, and an IT disaster recovery plan template aligns with NIST guidance, such as in SP 800-34 at https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-34r1.pdf.

Reflecting on a portfolio activity simulating an audit for a toy company expanding online, it highlighted real-world application: scoping IT assets, risk assessments for compliance with EU regulations and payment processing, and checklists for controls like MFA or data encryption. This reinforced how frameworks and principles not only mitigate risks but also support growth without compromising security.

---

### Key Takeaways

- Security analyst responsibilities: Safeguard organizations and individuals from data breaches impacting financial stability and reputations through daily protective tasks.
- Purpose of security frameworks: Guidelines for mitigating risks to data and privacy, aiding in policy creation for virtual and physical security, including breach prevention, detection, and response.
- Human element in security: Employees as primary threats; frameworks emphasize awareness, red flag recognition, and quick reporting to minimize breaches.
- Types of security controls: Encryption for confidentiality; authentication (e.g., MFA, biometrics) for identity verification; authorization for access permissions.
- CIA triad components: Confidentiality (need-to-know access); integrity (data accuracy and reliability); availability (timely access for authorized users).
- Application of CIA triad: In banks, protect personal info (confidentiality), verify identity on unusual activity (integrity), ensure web access with validation (availability).
- Specific frameworks: NIST CSF (voluntary standards for risk management); NIST SP 800-53 (federal info systems protection); Cyber Threat Framework (common language for threats); ISO/IEC 27001 (info security management for assets).
- Examples of physical controls: Gates, fences, locks; security guards; CCTV and motion detectors; access cards.
- Examples of technical controls: Firewalls; MFA; antivirus software.
- Examples of administrative controls: Separation of duties; authorization; asset classification.
- NIST CSF core functions: Identify (manage risks to people/assets); Protect (implement policies/tools); Detect (identify incidents efficiently); Respond (contain/analyze incidents, improve processes); Recover (restore systems/data).
- OWASP security principles: Minimize attack surface (disable unused features, strong passwords); least privilege (minimum access for tasks); defense in depth (layered controls); separation of duties (distribute privileges); keep security simple (avoid complexity); fix issues correctly (root cause analysis, testing).
- Additional OWASP principles: Establish secure defaults (optimal security as default); fail securely (default to secure on failure); don't trust services (verify third-party security); avoid security by obscurity (rely on robust factors).
- Security audit elements: Define scope (criteria like people/assets/policies); set goals (security objectives); conduct risk assessment (identify threats/vulnerabilities); complete controls assessment (evaluate effectiveness); assess compliance (e.g., GDPR, PCI DSS); communicate results (findings, recommendations).
- Audit checklist areas: Identify scope (assess assets, policies); complete risk assessment (budget/processes/standards); conduct audit (security of assets); create mitigation plan (lower risks/costs); communicate to stakeholders (detailed reports).
- Glossary terms: Asset (valuable item); attack vectors (penetration paths); authentication (identity verification); authorization (access granting); availability (accessible data); biometrics (physical characteristics); confidentiality (authorized access); CIA triad (risk model); detect (incident identification); encryption (data encoding); govern (strategy oversight); identify (risk management); integrity (reliable data); NIST CSF (risk management practices); NIST SP 800-53 (federal controls); OWASP (software security focus); protect (threat mitigation); recover (system restoration); respond (incident handling); risk (CIA impact); security audit (controls review); security controls (risk safeguards); security frameworks (risk plans); security posture (defense management); threat (negative impact event).

---






























