# Threat Modelling

---

Threat modelling is a systematic process for identifying, analysing, prioritising, and mitigating potential 
security threats within an organisation. It involves examining interconnected systems, applications, 
infrastructure, and existing vulnerabilities to understand possible attack scenarios and establish appropriate
security controls. Its primary objective is to reduce organisational risk exposure while strengthening 
security resilience.

A threat, vulnerability, and risk represent three distinct concepts. A threat is a potential event, actor, 
or occurrence capable of compromising the confidentiality, integrity, or availability of information. A 
vulnerability is a weakness in a system, application, or process that a threat could exploit. Risk represents
the possibility and potential impact of a successful exploitation. The house analogy makes these distinctions
easier to understand: burglary represents the threat, broken locks or open windows represent vulnerabilities,
and the likelihood of a burglary represents risk.

A threat modelling exercise begins by defining its scope and identifying the systems, applications, and 
networks under assessment. Asset identification follows, involving architectural diagrams, system 
dependencies, and an assessment of critical information such as customer data, intellectual property, and 
financial records. Potential threats are then identified, including cyber attacks, physical attacks, social 
engineering, and insider threats. Existing vulnerabilities and security controls are analysed before risks 
are prioritised according to likelihood and impact. Appropriate countermeasures are implemented, including 
access controls, system updates, and vulnerability assessments. Continuous monitoring and evaluation determine
whether the implemented measures have successfully mitigated or eliminated identified risks.

Threat modelling requires collaboration across organisational teams. The Security Team, including red and 
blue teams, generally leads the exercise and oversees threat identification, mitigation, implementation, and 
validation. Development Teams incorporate security into the development lifecycle. IT and Operations Teams 
contribute knowledge of networks, servers, configurations, and application integrations. Governance, Risk and
Compliance (GRC) Teams align the exercise with regulatory requirements and internal policies. Business 
Stakeholders identify critical assets, business processes, and organisational risk tolerance, while End Users
provide insight into vulnerabilities arising from actual system interactions and behaviour.

Attack trees provide another method of representing potential threats. They use a hierarchical structure in 
which the root node represents the attacker's primary objective, while child nodes describe possible 
strategies and their underlying actions. Attack paths provide a related representation, illustrating 
interconnected vulnerabilities and the sequence of actions an adversary could follow from an initial entry 
point towards the intended objective.

MITRE ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge) is a globally accessible knowledge 
base developed by the MITRE Corporation. It organises adversarial behaviour into tactics, representing 
high-level objectives, and techniques, describing the methods used to achieve them. Its technique pages 
provide descriptions, procedure examples, mitigations, detection guidance, and references. The Exploit 
Public-Facing Application technique illustrates how this information can support the understanding and 
assessment of adversary behaviour.

MITRE ATT&CK can be incorporated into threat modelling by mapping identified threats to corresponding tactics
and techniques. This introduces an additional mapping stage after threat identification. The associated 
technique descriptions, real-world procedure examples, mitigations, and detection strategies provide 
additional context for evaluating organisational exposure. The framework also supports attack-path 
identification based on infrastructure, threat-scenario development using known threat groups, and 
prioritisation of vulnerability remediation.

The MITRE ATT&CK Navigator is an open-source, web-based application for visualising and navigating ATT&CK 
matrices. It allows security teams to create customised layers containing techniques relevant to particular 
environments and threat scenarios. The three available matrices are Enterprise, covering enterprise 
networks; Mobile, covering smartphones and tablets; and Industrial Control Systems (ICS), covering systems
used in critical infrastructure.

Creating a layer begins with selecting the appropriate matrix. The Selection Controls panel provides 
search and selection capabilities for techniques, threat groups, software, mitigations, campaigns, and data
sources. Searching for APT41 under Threat Groups highlights techniques attributed to that group. Selected 
techniques can be annotated together, while individual techniques can also be managed through their context
menu.

Layer Controls provide export options in JavaScript Object Notation (JSON), Excel, and Scalable Vector 
Graphics (SVG) formats. Exported layers preserve selected techniques and annotations and can be imported 
again. Additional controls filter techniques by platforms, sort them alphabetically or by numerical score, 
and expand underlying sub-techniques.

Technique Controls provide options to disable selected techniques, change background colours, assign scores,
add comments, attach external links, and apply custom metadata. Existing annotations can also be cleared from
selected techniques.

A financial services scenario demonstrates how ATT&CK Navigator can support practical threat modelling.
The organisation uses Google Cloud Platform (GCP), an internally developed online banking platform, and a 
Customer Relationship Management (CRM) system. Its critical assets include customer financial data, 
transaction records, and Personally Identifiable Information (PII). Relevant threat groups include APT28 
(Fancy Bear), APT29 (Cozy Bear), Carbanak, FIN7 (Carbanak/Fancy Bear), and Lazarus Group.

Four techniques highlighted for prioritisation are Exploit Public-Facing Application (T1190), Exploitation 
for Privilege Escalation (T1068), Data from Cloud Storage (T1530), and Network Denial of Service (T1498). 
Their potential consequences include unauthorised application access, privilege escalation, exposure of 
cloud-stored information, and disruption of service availability. Identifying these techniques provides a 
basis for selecting remediation measures and reducing the organisation's attack surface.

The DREAD framework is a risk assessment model developed by Microsoft for evaluating and prioritising 
security threats and vulnerabilities. Its five components are Damage, Reproducibility, Exploitability, 
Affected Users, and Discoverability. Damage measures the potential consequences of successful exploitation.
Reproducibility considers how easily an attack can be repeated. Exploitability evaluates the technical 
difficulty, required skills, available tools, and time involved. Affected Users measures the portion of the 
user population impacted, while Discoverability considers how easily an attacker can identify the 
vulnerability.

DREAD commonly uses qualitative scoring from 1 to 10 for each category, with the average producing the 
overall risk rating. Because the assessment depends heavily on analyst interpretation, consistent scoring 
definitions, supporting examples, and collaboration between teams help improve reliability. Combining DREAD
with other risk assessment methodologies and periodically reviewing the chosen approaches also helps maintain
consistency with organisational requirements.

A software company assessing a newly developed website provides an example of DREAD scoring. Its guidelines 
use reference scores of 2.5, 5, 7.5, and 10. A complete data leak represents the highest Damage rating, while
minimal infrastructure information disclosure represents a lower rating. Reproducibility considers the 
technical expertise and preparation needed to repeat an exploit. Exploitability considers the availability 
and complexity of public exploits, including whether a reliable Metasploit module exists. Affected Users 
ranges from a small subset to the entire user base. Discoverability ranges from vulnerabilities requiring 
substantial investigation to those detectable through commonly available scanning tools.

Three vulnerability examples demonstrate the resulting assessments. Unauthenticated Remote Code Execution 
receives an overall score of 8, with Damage 10, Reproducibility 7.5, Exploitability 10, Affected Users 10, 
and Discoverability 2.5. Insecure Direct Object References (IDOR) in User Profiles receives 6.5, with 
respective component scores of 2.5, 7.5, 7.5, 10, and 5. Server Misconfiguration Leading to Information 
Disclosure receives 5, with component scores of 0, 10, 10, 0, and 5. The resulting ratings provide a basis 
for prioritising vulnerabilities and applying appropriate mitigations.

The STRIDE framework, also developed by Microsoft, categorises potential threats into six areas: Spoofing, 
Tampering, Repudiation, Information Disclosure, Denial of Service, and Elevation of Privilege. These 
categories correspond to violations of authentication, integrity, non-repudiation, confidentiality, 
availability, and authorisation, respectively. They provide a structured approach to examining security 
weaknesses in software, applications, and infrastructure.

Spoofing involves impersonating another user or system, such as sending an email under another person's 
identity or creating a fraudulent website to collect credentials. Tampering concerns unauthorised 
modification of data or code, including changing another user's password or installing backdoors with 
elevated access. Repudiation involves denying an action when adequate auditing or logging is unavailable, 
such as disputing a financial transaction without sufficient records.

Information Disclosure occurs when sensitive information becomes accessible without authorisation. Examples
include accessing a misconfigured database containing customer information or publicly exposed cloud storage
containing sensitive documents. Denial of Service involves disrupting availability, such as overwhelming a
web server with requests or deploying ransomware that prevents systems from accessing required resources. 
Elevation of Privilege occurs when an attacker obtains permissions beyond those intended, such as accessing 
an administrator console through a regular account or exploiting an unpatched system to obtain local 
administrator privileges.

STRIDE assessments can be represented through a checklist that maps individual scenarios against the six 
threat categories. A single scenario may affect multiple categories. Examples include spoofed emails 
involving inadequate gateway security and logging, excessive requests against a web server without load 
balancing, SQL injection, unauthorised access to public cloud storage containing customer information, and 
local privilege escalation followed by system configuration changes for persistent backdoor access.

Implementing STRIDE begins with system decomposition, where applications, networks, and data flows are 
separated into components and their trust boundaries and attack surfaces are identified. Each component is 
then assessed against the six STRIDE categories. Identified threats are evaluated according to likelihood 
and impact before appropriate countermeasures are developed. For email spoofing, the source identifies 
Domain-based Message Authentication, Reporting and Conformance (DMARC), DomainKeys Identified Mail (DKIM),
and Sender Policy Framework (SPF) as relevant email authentication and validation mechanisms. Implemented 
controls are subsequently tested through penetration testing, code reviews, or security audits. The threat 
model must also be reviewed and updated as systems change and new threats emerge.

An e-commerce payment processing system provides an application scenario for STRIDE. The exercise involves
the Development Team, System Architecture Team, Security Team, Business Stakeholder Team, and Network 
Infrastructure Team. Developers build the applications, architects design the cloud infrastructure, security
personnel provide threat and mitigation expertise, business stakeholders identify critical assets and 
objectives, and network infrastructure personnel manage servers and other critical systems.

PASTA, or Process for Attack Simulation and Threat Analysis, is a structured, risk-centric threat modelling
framework. Created by Tony UcedaVélez and Marco Morana, it was introduced in their 2015 book, *Risk Centric
Threat Modeling: Process for Attack Simulation and Threat Analysis*. Its seven-step methodology is intended
to help organisations identify threats, evaluate attack scenarios, assess potential impact, and prioritise 
remediation according to business and security objectives.

The methodology begins by defining objectives, including the systems under assessment, security goals, and 
compliance requirements. Technical scope definition follows through an inventory of hardware, software, and
data, together with an examination of architecture, dependencies, and data flows. Application decomposition
then identifies system components, entry points, trust boundaries, attack surfaces, and user roles and 
privileges.

Threat analysis considers external attackers, insider threats, and accidental exposures, drawing on threat 
classification frameworks and attack libraries. Vulnerability and weakness analysis examines issues such as 
misconfigurations, software bugs, and unpatched systems. Static and dynamic code analysis, vulnerability
assessments, and penetration testing can support this stage. Attack analysis involves developing and 
simulating realistic attack scenarios to evaluate their likelihood and impact. Attack trees can be used to
document these scenarios and check that relevant use cases are covered. Finally, risk and impact analysis
determines the severity of identified threats and guides the selection of effective, cost-efficient 
countermeasures, including software updates, patches, and access controls.

PASTA benefits from clear objectives, documented compliance requirements, comprehensive asset inventories,
and detailed architectural understanding. Its application decomposition should account for entry points, 
trust boundaries, attack surfaces, data flows, and user flows. Threat analysis should consider different 
threat sources and use threat intelligence feeds and industry practices to remain aware of emerging threats.
Vulnerability analysis requires appropriate assessment tools and prompt remediation of known weaknesses. 
Attack scenarios should be realistic and aligned with the exercise's objectives, while risk decisions should
reflect organisational risk tolerance and security requirements.

PASTA supports collaboration between developers, architects, security professionals, and other stakeholders. 
Its adaptability allows organisations to align threat modelling with business objectives and compliance 
requirements. Its systematic, risk-centric approach provides a means of examining the wider organisational 
risk landscape, although the methodology should be adapted to the organisation's particular needs.

An online banking platform serving users across the Asia-Pacific region provides the practical PASTA scenario.
The exercise involves the Development Team, System Architecture Team, Security Team, and Business Stakeholder 
Team. Their responsibilities include application development, cloud architecture design, security expertise,
and identifying critical assets and business processes. The seven-step methodology guides the selection of 
appropriate stakeholders and the collection of information needed for the exercise.

The four frameworks serve different purposes. MITRE ATT&CK maps adversary tactics and techniques, supporting 
the assessment of existing security controls against known attack behaviour. DREAD provides numerical 
qualitative risk assessments to help prioritise threats and remediation. STRIDE offers a structured approach
to categorising software and system threats and selecting corresponding security controls. PASTA provides 
a risk-centric methodology that incorporates business objectives and organisational context.

Together, these approaches support threat awareness, vulnerability identification, risk prioritisation, 
security control optimisation, and continuous improvement. Their application helps organisations develop 
a more structured understanding of potential attacks and improve their ability to mitigate security risks.

---

## **Extracted Tables**

**Threat, Vulnerability, and Risk**

| Type          | Definition                                                                                                   |
| ------------- | ------------------------------------------------------------------------------------------------------------ |
| Threat        | A potential event, actor, or occurrence capable of compromising confidentiality, integrity, or availability. |
| Vulnerability | A weakness or flaw in a system, application, or process that can be exploited.                               |
| Risk          | The possibility and potential consequences of a threat exploiting a vulnerability.                           |

**House Analogy**

| Type          | Analogy                                                                          |
| ------------- | -------------------------------------------------------------------------------- |
| Threat        | Someone breaking into a house and stealing belongings.                           |
| Vulnerability | Broken locks or open windows.                                                    |
| Risk          | Likelihood of burglary due to neighbourhood crime or absence of an alarm system. |

**Organisational Teams**

| Team                                 | Role and Purpose                                                                                      |
| ------------------------------------ | ----------------------------------------------------------------------------------------------------- |
| Security Team                        | Leads threat modelling and manages threat identification, mitigation, implementation, and monitoring. |
| Development Team                     | Builds secure systems and applications throughout the development lifecycle.                          |
| IT and Operations Team               | Manages infrastructure, networks, servers, configurations, and integrations.                          |
| Governance, Risk and Compliance Team | Oversees regulatory compliance and alignment with organisational risk objectives.                     |
| Business Stakeholders                | Identify critical assets, business processes, and risk tolerance.                                     |
| End Users                            | Provide insight into vulnerabilities arising from actual system usage.                                |

---

**DREAD Categories**

| Category        | Definition                                                                            |
| --------------- | ------------------------------------------------------------------------------------- |
| Damage          | Potential harm, including data loss, downtime, or reputational damage.                |
| Reproducibility | Ease of repeating a successful exploitation.                                          |
| Exploitability  | Difficulty of exploiting a vulnerability, including required skills, tools, and time. |
| Affected Users  | Number or proportion of users impacted.                                               |
| Discoverability | Ease of identifying the vulnerability.                                                |

**DREAD Scoring Guidelines**

| Category        | 2.5                                           | 5                                      | 7.5                                 | 10                                             |
| --------------- | --------------------------------------------- | -------------------------------------- | ----------------------------------- | ---------------------------------------------- |
| Damage          | Minimal infrastructure information disclosure | Minimal client-data disclosure         | Limited PII leak                    | Complete data leak                             |
| Reproducibility | Multiple attack vectors requiring expertise   | Minor customisation of public exploits | Little technical expertise required | Public exploits easily reproduce the attack    |
| Exploitability  | Few public exploits; custom scripts needed    | Complicated public exploit scripts     | Minimal technical skills required   | Reliable Metasploit module exists              |
| Affected Users  | Almost none or a small subset                 | Around 10%                             | More than half                      | All users                                      |
| Discoverability | Significant effort required                   | Manual vulnerability verification      | Public scanning scripts available   | Common scanning tools detect the vulnerability |

**DREAD Vulnerability Assessments**

| Vulnerability                                             | Damage | Reproducibility | Exploitability | Affected Users | Discoverability | Overall |
| --------------------------------------------------------- | ------ | --------------- | -------------- | -------------- | --------------- | ------- |
| Unauthenticated Remote Code Execution                     | 10     | 7.5             | 10             | 10             | 2.5             | 8       |
| IDOR in User Profiles                                     | 2.5    | 7.5             | 7.5            | 10             | 5               | 6.5     |
| Server Misconfiguration Leading to Information Disclosure | 0      | 10              | 10             | 0              | 5               | 5       |

---

**STRIDE Categories**

| Category               | Definition                                                 | Policy Violated |
| ---------------------- | ---------------------------------------------------------- | --------------- |
| Spoofing               | Unauthorised impersonation of a user or system.            | Authentication  |
| Tampering              | Unauthorised modification of data or code.                 | Integrity       |
| Repudiation            | Denying an action due to insufficient auditing or logging. | Non-repudiation |
| Information Disclosure | Unauthorised access to sensitive information.              | Confidentiality |
| Denial of Service      | Disrupting availability for legitimate users.              | Availability    |
| Elevation of Privilege | Obtaining unauthorised access privileges.                  | Authorisation   |

**STRIDE Scenario Assessment**

A checkmark indicates the applicable threat category.

| Scenario                                                                 | Spoofing | Tampering | Repudiation | Information Disclosure | Denial of Service | Elevation of Privilege |
| ------------------------------------------------------------------------ | -------- | --------- | ----------- | ---------------------- | ----------------- | ---------------------- |
| Spoofed email with inadequate gateway security and logging               | ✓        |           | ✓           |                        |                   |                        |
| Flooding a web server without load balancing                             |          |           |             |                        | ✓                 |                        |
| SQL injection vulnerability                                              |          | ✓         |             | ✓                      |                   |                        |
| Accessing public cloud storage containing customer data                  |          |           |             | ✓                      | ✓                 |                        |
| Local privilege escalation and persistent backdoor configuration changes |          | ✓         |             |                        |                   | ✓                      |

**STRIDE Implementation Teams**

| Team                        | Responsibilities                                                                   |
| --------------------------- | ---------------------------------------------------------------------------------- |
| Development Team            | Builds organisational systems and applications.                                    |
| System Architecture Team    | Designs cloud service architecture.                                                |
| Security Team               | Provides threat, vulnerability, and mitigation expertise.                          |
| Business Stakeholder Team   | Identifies critical assets and business processes and ensures strategic alignment. |
| Network Infrastructure Team | Manages networks, servers, and critical systems.                                   |

**PASTA Implementation Teams**

| Team                      | Responsibilities                                                                   |
| ------------------------- | ---------------------------------------------------------------------------------- |
| Development Team          | Builds organisational systems and applications.                                    |
| System Architecture Team  | Designs cloud service architecture.                                                |
| Security Team             | Provides threat, vulnerability, and mitigation expertise.                          |
| Business Stakeholder Team | Identifies critical assets and business processes and ensures strategic alignment. |

**Threat Modelling Framework Comparison**

| Framework    | Primary Application                                                                                 |
| ------------ | --------------------------------------------------------------------------------------------------- |
| MITRE ATT&CK | Maps adversary tactics and techniques and evaluates existing security controls.                     |
| DREAD        | Qualitatively scores threats and prioritises remediation through numerical assessments.             |
| STRIDE       | Categorises software and system threats and identifies corresponding security controls.             |
| PASTA        | Conducts risk-centric threat modelling aligned with business objectives and organisational context. |

---

## **Key Takeaways**

- Threat modelling aims to reduce organisational risk through systematic identification, assessment,
  prioritisation, and mitigation.
- Understand the distinction between threats, vulnerabilities, and risks before conducting an assessment.
- Follow the high-level process: define scope, identify assets, identify threats, analyse vulnerabilities,
  prioritise risks, implement countermeasures, and continuously monitor results.
- Involve security, development, IT operations, GRC, business stakeholders, and end users.
- Use attack trees to represent attacker objectives and break them into smaller actions. Use attack paths to
  illustrate interconnected vulnerabilities.
- Use MITRE ATT&CK to map identified threats to adversary tactics and techniques.
- Use ATT&CK Navigator to create layers, search techniques, select threat groups, filter platforms, expand
  sub-techniques, score techniques, and annotate findings.
- Export ATT&CK Navigator layers in JSON, Excel, or SVG formats.
- Consider infrastructure, threat groups, and critical assets when prioritising ATT&CK techniques.
- Apply DREAD using its five categories: Damage, Reproducibility, Exploitability, Affected Users, and
  Discoverability.
- Standardise DREAD scoring guidelines, encourage cross-team discussions, and combine the framework with
  other assessment methodologies.
- Prioritise vulnerabilities according to their calculated DREAD ratings and implement appropriate
  mitigations.
- Apply STRIDE systematically to each system component, examining all six threat categories.
- Use DMARC, DKIM, and SPF when addressing email spoofing scenarios.
- Validate STRIDE countermeasures through penetration testing, code reviews, and security audits.
- Revisit threat models as systems evolve and new threats emerge.
- Apply PASTA through its seven stages: define objectives, define technical scope, decompose the application,
  analyse threats, analyse vulnerabilities and weaknesses, analyse attacks, and conduct risk and impact
  analysis.
- Use threat intelligence, static and dynamic code analysis, vulnerability scanning, penetration testing, and
   attack trees where appropriate within PASTA.
- Align PASTA countermeasures with organisational risk tolerance, security objectives, and business
  requirements.
- Select frameworks according to their intended applications: ATT&CK for adversary behaviour, DREAD for risk
   scoring, STRIDE for threat categorisation, and PASTA for business-aligned risk analysis.
- Treat two details in the source as assessment caveats: the DREAD scoring guidance specifies a 1–10 range
  but its worked example uses 0; the PASTA seventh-step description discusses countermeasures under the Risk
   and Impact Analysis heading.

---

**Reference resources**

- MITRE ATT&CK: [https://attack.mitre.org/](https://attack.mitre.org/?utm_source=chatgpt.com)&#x20;

  [image](https://www.google.com/s2/favicons?domain=https://attack.mitre.org\&sz=32)

  MITRE ATT&CK

- MITRE ATT&CK Enterprise Matrix: [https://attack.mitre.org/matrices/enterprise/](https://attack.mitre.org/matrices/enterprise/?utm_source=chatgpt.com)&#x20;

  [image](https://www.google.com/s2/favicons?domain=https://attack.mitre.org\&sz=32)

  MITRE ATT&CK®

- MITRE ATT&CK Navigator: [https://mitre-attack.github.io/attack-navigator/](https://mitre-attack.github.io/attack-navigator/?utm_source=chatgpt.com)

- PASTA reference book: [https://onlinelibrary.wiley.com/book/10.1002/9781118988374](https://onlinelibrary.wiley.com/book/10.1002/9781118988374?utm_source=chatgpt.com)&#x20;

  [image](https://www.google.com/s2/favicons?domain=https://onlinelibrary.wiley.com\&sz=32)

  Wiley Online Books

---





