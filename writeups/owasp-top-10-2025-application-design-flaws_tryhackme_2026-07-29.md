# OWASP Top 10 2025: Application Design Flaws 

---

This journal entry records notes from the TryHackMe room covering OWASP Top 10 2025 categories tied to failures in architecture and 
system design. The material examines AS02 Security Misconfigurations, AS03 Software Supply Chain Failures, AS04 Cryptographic Failures 
and AS06 Insecure Design, with supporting practical challenges that require deploying the lab machine and connecting through the 
AttackBox or an equivalent machine linked to the platform.

Security misconfigurations arise when systems, servers or applications launch with unsafe defaults, incomplete settings or exposed 
services. These represent environment, software or network setup errors rather than flaws in application code and they open 
straightforward paths for attackers. Minor instances can still leak sensitive data, enable privilege escalation or establish an initial
foothold. Modern stacks that incorporate cloud services and third-party APIs heighten the impact, since a single exposed administration
interface, open storage container or incorrect permission set can undermine the whole environment. The 2017 Uber incident illustrated
the point when a publicly accessible backup bucket released driver and rider information that could be downloaded without any 
credentials. Typical indicators include unchanged default credentials, internet-facing services that serve no purpose, misconfigured 
cloud storage permissions on Azure Blob or GCP buckets, missing authentication, verbose error output that reveals stack traces, 
unpatched frameworks or containers carrying known weaknesses, and unprotected endpoints. Prevention centres on hardening defaults and
removing unused features, enforcing strong authentication together with least privilege, restricting network exposure and segmenting
sensitive assets, applying timely patches, suppressing system details in error responses, conducting regular cloud configuration 
audits, locking down endpoints with monitoring, and embedding configuration reviews plus automated checks into the deployment pipeline.
The associated challenge is reached at <TARGET_IP>:5002, where residual traces remain inside the User Management APIs.

Software supply chain failures occur when an application depends on components, libraries, services or models that are themselves 
compromised, outdated or insufficiently verified. The weakness lies outside the application’s own code and resides in the external 
software and tooling it consumes. Attackers abuse these links to introduce malicious code, circumvent controls or extract data. Because 
contemporary applications assemble large numbers of third-party packages, APIs and models, a single tainted dependency can grant access
without ever touching the proprietary codebase; such attacks scale automatically and prove difficult to detect. The 2021 SolarWinds 
Orion event demonstrated the risk when malicious code was inserted into a trusted update that thousands of organizations installed 
automatically. The same pattern appears with unverified third-party models or fine-tuned datasets that embed hidden behaviors, 
backdoors or biased outputs capable of compromising systems or leaking data. Recurring patterns include reliance on unmaintained 
libraries, automatic installation of unverified updates, insufficient monitoring of third-party models, build processes open to 
tampering, weak provenance tracking and absent post-deployment vulnerability monitoring. Protection requires verification of every 
third-party component before use, continuous monitoring and patching of dependencies, cryptographic signing and auditing of updates, 
locking down build pipelines, maintaining provenance and license records, runtime monitoring for anomalous behavior, and integration 
of supply-chain threat modelling across testing, deployment and update workflows. The challenge is located at <TARGET_IP>:5003, where 
outdated code imports an old lib/vulnerable_utils.py component that must be examined.

Cryptographic failures result from incorrect or absent use of encryption, encompassing weak algorithms, hard-coded keys, inadequate 
key handling or failure to protect sensitive data. These defects allow attackers to reach information that should remain private. 
Web applications depend on cryptography for traffic protection, stored-data confidentiality, identity verification and secret 
safeguarding; when the controls fail, passwords, tokens or personal details become exposed and can lead to account takeover or broader
breaches. Exploitation routes include man-in-the-middle interception, brute-force attacks against weak keys, or simple discovery of 
unprotected secrets. Common manifestations are deprecated algorithms such as SHA-1 or ECB mode, secrets embedded in source or 
configuration, poor key-rotation practices, unencrypted data at rest or in transit, self-signed or invalid certificates, and systems
that handle model parameters or sensitive inputs without proper secret management. Countermeasures consist of adopting modern 
algorithms such as AES-GCM or ChaCha20-Poly1305 and enforcing TLS 1.3 with valid certificates, employing secure key-management services
including Azure Key Vault or HashiCorp Vault, rotating secrets according to defined crypto periods, documenting key-lifecycle policies,
maintaining an inventory of certificates and keys with their owners, ensuring models never expose unencrypted secrets, and examining
the weak implementation present in the room’s web application. The practical task is available at <TARGET_IP>:5004 and requires
locating the key needed to decrypt a protected file.

Insecure design is present when flawed logic or architecture is embedded from the outset through omitted threat modelling, missing 
design requirements or simple oversight. The introduction of assistants intensifies the problem because developers may treat models
as inherently safe, correct or predictable and may accept generated code without scrutiny. When a system can generate queries, produce 
code or classify users without constraints, the architectural risk is already built in. Clubhouse supplied a concrete illustration: 
its early design assumed interaction only through the mobile application, yet the backend lacked proper authentication, allowing direct 
queries of user data, room information and private conversations that dismantled the privacy premise. Design flaws cannot be patched 
after the fact; remediation demands rethinking workflows, logic and trust boundaries. Characteristic insecure patterns in 2025 include
weak business-logic controls around recovery or approval flows, incorrect assumptions about user or model behaviour, components granted
unchecked authority, missing guardrails for large language models and automation agents, debug bypasses left in production, and absence
of consistent abuse-case review. In the current era, prompt injection arises when user input is mixed with system prompts, enabling 
context hijacking or extraction of hidden data; blind trust in model output produces systems that act without validation, making human
review essential; poisoned models obtained from unverified sources or fine-tuned on unsafe data can embed backdoors that compromise 
the environment from within. Secure design practices treat every model as untrusted until proven otherwise, validate and filter all 
model inputs and outputs, separate system prompts from user content, keep sensitive data out of prompts under strict controls, require
human review for high-risk actions, log model provenance while applying differential privacy where appropriate, incorporate threat 
modelling for prompt attacks, inference risks, agent misuse and supply-chain compromise throughout the design process, embed threat 
modelling at every development stage, define security requirements before implementation, apply least privilege across users, APIs and
services, ensure consistent authentication, authorisation and session management, keep dependencies verified and current, and 
continuously monitor for logic flaws and emergent risks. The final challenge resides at <TARGET_IP>:5005 and questions whether access
was assumed to be limited to mobile devices only.

Across AS02, AS03, AS04 and AS06 the common root is weak foundations. Security cannot be bolted on after the fact; robust systems 
begin with explicit security requirements, realistic threat assumptions, controlled configurations, vetted dependencies and sound 
cryptographic choices. Defaults should be treated with suspicion, every dependency regarded as a potential risk, and designs kept 
simple enough to reason about. Establishing these elements early avoids a prolonged series of preventable incidents. Further material
continues in the subsequent room at https://tryhackme.com/jr/owasptop102025insecuredatahandling.

---

### Key Takeaways
- Harden default configurations and remove unused features or services; enforce strong authentication and least privilege; limit
  network exposure and segment sensitive resources; keep software, frameworks and containers patched; hide stack traces from error
  messages; audit cloud configurations regularly; secure endpoints with access controls and monitoring; integrate configuration reviews
   into the deployment pipeline.
- Verify all third-party components, libraries and models before use; monitor and patch dependencies regularly; sign, verify and audit
 software updates; lock down build processes against tampering; track provenance and licensing; implement runtime monitoring for
unusual behaviour; integrate supply-chain threat modelling into testing, deployment and update workflows.
- Prefer strong modern algorithms such as AES-GCM or ChaCha20-Poly1305 and enforce TLS 1.3 with valid certificates; use secure
  key-management services; rotate secrets according to defined crypto periods; document key-lifecycle policies; maintain an inventory
  of certificates and keys; ensure models never expose unencrypted secrets.
- Treat every model as untrusted until proven otherwise; validate and filter model inputs and outputs; separate system prompts from
  user content; keep sensitive data out of prompts under strict controls; require human review for high-risk actions; log model
  provenance and apply differential privacy; include AI-specific threat modelling throughout design; build threat modelling into every
   development stage; define clear security requirements before implementation; apply least privilege across users, APIs and services;
   ensure proper authentication, authorisation and session management; keep dependencies verified and current; continuously monitor for
   logic flaws and emergent risks.

---





