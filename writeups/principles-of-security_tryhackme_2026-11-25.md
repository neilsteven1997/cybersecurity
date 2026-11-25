# Principles of Security

---

Defence in Depth is a security approach that protects an organisation's systems and data through multiple, independent layers of security. 
These layers provide redundancy, ensuring that the failure of one security measure does not leave the entire environment unprotected. The 
frameworks, protocols, and controls discussed in this material contribute to this approach.

The CIA Triad, consisting of Confidentiality, Integrity, and Availability, is a foundational information security model used when developing 
security policies. Its origins extend back to 1998, reflecting the broader scope of information security beyond cybersecurity, including 
physical records, filing systems, and information storage. The three principles operate together, and a security policy must address all of 
them to provide effective protection. The National Institute of Standards and Technology (NIST) is referenced as a resource for this model.

Confidentiality protects information against unauthorized access and misuse. Organisations hold different categories of sensitive information,
including employee records and accounting documents. Access restrictions should reflect the sensitivity of the information. For example, human
resources records may be restricted to vetted HR administrators, while accounting documents may require less restrictive controls. Governments 
also apply classification systems such as Top Secret, Classified, and Unclassified to distinguish information sensitivity.

Integrity ensures that information remains accurate and consistent unless an authorized modification takes place. Data can be compromised 
through careless handling, system errors, or unauthorized access. Integrity must be maintained during storage, transmission, and use. Access 
controls and strong authentication help prevent unauthorized modifications, while hash verification and digital signatures provide ways to
verify transaction authenticity and detect file alteration or corruption.

Availability ensures that information and systems remain accessible to authorized users when required. Organisations may establish uptime 
targets, such as 99.99%, through Service Level Agreements (SLAs). System downtime can damage an organisation's reputation and cause financial
losses. Availability depends on reliable and tested server hardware, redundant technologies and services that can take over when primary 
systems fail, and security protocols that protect infrastructure against attacks.

Access privileges must be assigned according to an individual's organisational responsibilities and the sensitivity of the information they
need. Two concepts support this process: Privileged Identity Management (PIM) and Privileged Access Management (PAM). PIM translates an 
individual's organisational role into an appropriate system access role. PAM manages the privileges associated with that role and covers 
additional controls, including password management, auditing policies, and reducing the system's attack surface. The principle of least
privilege requires users to receive only the permissions necessary to perform their assigned duties.

Security models provide structured methods for implementing the CIA Triad. An information system refers to any system or technology that stores
information. Two models covered in this material are Bell-La Padula and Biba.

The Bell-La Padula Model focuses on confidentiality and assumes a well-defined organisational hierarchy with established responsibilities. It 
controls access to data objects according to a need-to-know principle through the rule "no write down, no read up." Its advantages include 
straightforward implementation, simplicity, and the ability to reflect real organisational hierarchies. However, users may still discover that 
restricted objects exist, even when they cannot access their contents. The model also depends heavily on organisational trust. It is commonly 
associated with government and military environments, where personnel undergo vetting to assess their backgrounds and suitability before being
trusted with access.

The Biba Model focuses on integrity and applies access restrictions to subjects, representing users, and objects, representing data. Its rule 
is "no write up, no read down." Subjects may create or modify objects at or below their assigned level but can only read objects above their 
level. The model is relatively simple to implement and addresses limitations associated with Bell-La Padula by focusing on data integrity. 
However, managing numerous access levels and objects can introduce oversight, while restrictions may delay business operations. A hospital 
example illustrates this limitation: a doctor may be unable to read notes created by a nurse. Biba is particularly relevant where integrity
takes priority over confidentiality, such as software development environments in which developers receive access only to the code and resources
necessary for their responsibilities, without automatically gaining access to critical databases.

Threat modelling involves examining, improving, and testing the security controls protecting an organisation's information technology 
infrastructure and services. A central part of the process is identifying potential threats and vulnerabilities affecting applications and
systems. Its general workflow consists of preparation, identification, mitigation, and review. Effective threat modelling requires continuous
assessment and collaboration with a dedicated team, supported by threat intelligence, asset identification, mitigation capabilities, and risk
assessment.

Two frameworks introduced for threat modelling are STRIDE and PASTA (Process for Attack Simulation and Threat Analysis). STRIDE was developed
by two Microsoft security researchers in 1999 and identifies six categories of threats. Spoofing occurs when an attacker falsely presents 
themselves as another identity; authentication mechanisms, access keys, and cryptographic signatures can help address it. Tampering involves 
unauthorized modification of information, making anti-tampering controls necessary to preserve integrity. Repudiation concerns the denial of
actions, which can be addressed through activity logging. Information Disclosure occurs when applications expose information that should only
be accessible to its intended owner. Denial of Service involves abusing application or service resources until normal operations are disrupted.
Elevation of Privilege occurs when a user gains permissions beyond their authorized level, potentially obtaining administrator access and 
enabling further exploitation or information disclosure.

A security breach is classified as an incident. Incident Response (IR) covers the actions taken to address and remediate such events. Incidents
are evaluated according to urgency and impact. Urgency depends on the nature of the attack, while impact considers the affected systems and 
consequences for business operations.

A Computer Security Incident Response Team (CSIRT) is a designated group of employees with technical knowledge of the organisation's systems and 
the incident being handled. Incident Response follows six phases: Preparation establishes whether the necessary resources and plans are 
available. Identification determines whether the threat and responsible threat actor have been correctly identified. Containment limits the 
incident to prevent further impact on other systems or users. Eradication removes the active threat. Recovery reviews affected systems and 
restores normal business operations. Lessons Learned examines the incident to identify improvements, such as providing employees with better
phishing-awareness training when an incident originates from a phishing email.

---

### **Key Takeaways**

- Defence in Depth relies on multiple security layers to provide redundancy and protect organisational systems and information.
- The CIA Triad consists of Confidentiality, Integrity, and Availability. Effective security policies must address all three.
- Confidentiality protects sensitive information through appropriate access restrictions and classification.
- Integrity preserves information accuracy through access controls, authentication, hash verification, and digital signatures.
- Availability ensures authorized users can access systems and information when needed. Reliable hardware, redundancy, and security protocols
  support availability.
- PIM translates organisational responsibilities into system access roles, while PAM manages the privileges and policies associated with those
  roles.
- Apply the principle of least privilege by granting users only the permissions required for their duties.
- Bell-La Padula protects confidentiality through the rule **no write down, no read up**. It is particularly relevant to hierarchical government
   and military environments.
- Biba protects integrity through the rule **no write up, no read down**. It is useful in environments where data integrity takes priority.
- Threat modelling follows four general activities:
  - Preparation.
  - Identification of threats and vulnerabilities.
  - Mitigation.
  - Review.
- Effective threat modelling incorporates threat intelligence, asset identification, mitigation capabilities, and risk assessment.
- STRIDE identifies six threat categories:
  - Spoofing.
  - Tampering.
  - Repudiation.
  - Information Disclosure.
  - Denial of Service.
  - Elevation of Privilege.
- Incident Response evaluates incidents according to urgency and business impact and is coordinated by a CSIRT.
- The six Incident Response phases are:
  - **Preparation:** Establish available resources and response plans.
  - **Identification:** Determine the threat and threat actor.
  - **Containment:** Prevent the incident from spreading.
  - **Eradication:** Remove the active threat.
  - **Recovery:** Restore affected systems to normal operations.
  - **Lessons Learned:** Review the incident and identify improvements to prevent recurrence.

---


