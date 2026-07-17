# Security Principles

---

Security has become a common term that companies frequently invoke to describe their offerings, yet true security requires understanding
the specific adversary one faces. Protecting against casual access differs vastly from defending against sophisticated actors targeting 
high-value assets like technical designs worth millions, making it essential to profile the threat actor before selecting controls. 
Perfect security remains unattainable, so efforts focus on strengthening the overall posture to raise the bar for potential attackers. 

The core security functions revolve around the CIA triad: confidentiality, integrity, and availability. Confidentiality restricts data
access to authorized parties only. Integrity guarantees data remains unaltered and allows detection of any changes. Availability 
ensures systems and services remain accessible when required. These principles apply across scenarios, such as online shopping where 
credit card details demand confidentiality to prevent unauthorized disclosure, order details need integrity to stop shipping address 
tampering, and the platform itself must maintain availability for browsing and transactions. In healthcare, patient records similarly 
require strict confidentiality under legal mandates, integrity to avoid life-threatening treatment errors from altered data, and 
availability so practitioners can reference medical history during consultations. Emphasis on each element can vary; for instance, a 
public university announcement prioritizes integrity over confidentiality.

Extending beyond the triad, authenticity confirms data originates from the claimed source without fraud, while nonrepudiation prevents 
the source from denying their involvement. Both prove critical in domains like e-commerce, healthcare, and banking, where verifying an 
order for high-value items such as vehicles and ensuring the originator cannot later disavow it enables trustworthy operations.

The Parkerian Hexad, proposed by Donn Parker in 1998, expands these ideas with six elements: availability, utility, authenticity, 
confidentiality, possession, and integrity. Utility addresses whether information remains usable, as in the case of encrypted data 
where the decryption key is lost despite the storage media being intact. Possession protects against unauthorized seizure, copying, or
control, such as an adversary stealing a backup drive or deploying ransomware.

Attacks typically target systems through disclosure, alteration, or destruction/denial, which form the direct opposites of the CIA 
triad—disclosure against confidentiality, alteration against integrity, and destruction or denial against availability. In patient
record systems, disclosure via public leaks brings legal and reputational harm, alteration risks incorrect treatments, and denial
through database unavailability halts operations even in paperless environments. Balancing these functions is necessary, as 
overemphasizing confidentiality and integrity can impair availability, while maximizing availability may compromise the others.

Security models provide structured approaches to enforcing these properties. The Bell-LaPadula model prioritizes confidentiality 
through the simple security property (no read up), star security property (no write down), and discretionary security property using 
an access matrix. These rules effectively allow reading down and writing up between security levels. Limitations include challenges 
with file sharing. The Biba model targets integrity with its simple property (no read down) and star property (no write up), summarized
as read up and write down, contrasting directly with Bell-LaPadula. It struggles with insider threats. The Clark-Wilson model also 
focuses on integrity via constrained data items whose integrity must be preserved, unconstrained data items like inputs, transformation
procedures that maintain integrity during operations, and integrity verification procedures that validate state. Additional models 
exist, such as Brewer and Nash, Goguen-Meseguer, Sutherland, Graham-Denning, and Harrison-Ruzzo-Ullman.

---

**Extracted Tables**

| Subjects   | Object A     | Object B    |
|------------|--------------|-------------|
| Subject 1  | Write        | No access   |
| Subject 2  | Read/Write   | Read        |

---

Defence-in-depth, also known as multi-level security, layers multiple controls so that compromising one does not grant full access, 
much like securing a locked drawer inside a locked room within a locked building augmented by cameras. 

ISO/IEC 19249:2017 from the International Organization for Standardization and International Electrotechnical Commission catalogues 
architectural and design principles for secure systems. Its architectural principles include domain separation, which groups related 
components with shared security attributes such as processor privilege rings; layering, as seen in the OSI model or abstracted 
programming interfaces, to enforce and validate policies; encapsulation to hide implementations and control access through defined 
methods or APIs; redundancy to support availability and integrity, for example via dual power supplies or RAID 5; and virtualization 
for sandboxing and isolation in cloud environments. Design principles cover least privilege on a need-to-know basis, attack surface 
minimization by disabling unneeded services, centralized parameter validation to counter invalid inputs, centralized general security 
services like authentication servers while avoiding single points of failure, and preparing for error and exception handling with 
fail-safe designs that avoid leaking confidential details.

Regarding trust, the principle of trust but verify calls for ongoing validation through logging, intrusion detection, and prevention 
systems despite initial confidence in an entity. Zero trust treats trust itself as a vulnerability, assuming all entities adversarial 
until verified through repeated authentication and authorization, often via microsegmentation where network segments can be as granular
as individual hosts. This limits breach impact compared to traditional perimeter models.

Distinguishing related terms avoids confusion: a vulnerability represents a weakness, a threat is the potential danger exploiting that 
weakness, and risk evaluates the likelihood of exploitation combined with business impact. A hospital database with a known exploit 
illustrates a real threat requiring risk assessment and response.

The shared responsibility model gains importance in cloud environments, where responsibilities for hardware, networks, operating 
systems, and applications divide between provider and customer depending on the service type—full control in IaaS versus limited access
in SaaS. This framework clarifies duties for comprehensive security. 

---

## Key Takeaways
- Profile the specific adversary before implementing controls, as protections suitable for casual access differ from those needed
  against advanced persistent threats.
- Maintain balance among confidentiality, integrity, and availability, recognizing that extremes in one area can undermine the others.
- Apply defence-in-depth through multiple overlapping controls rather than relying on a single barrier.
- Follow ISO/IEC 19249 architectural principles: domain separation, layering, encapsulation, redundancy, and virtualization.
- Adhere to ISO/IEC 19249 design principles: least privilege, attack surface minimization, centralized parameter validation,
  centralized general security services, and preparation for error handling.
- Implement trust but verify with logging and automated monitoring, or adopt zero trust with continuous verification and
  microsegmentation where feasible to address insider risks.
- Differentiate vulnerability as weakness, threat as potential danger, and risk as likelihood times impact.

---



