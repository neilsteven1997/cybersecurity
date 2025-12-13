# Networking Secure Protocols 
---
## TLS

The historical reliance on cleartext protocols meant that placing a network interface into promiscuous mode, thereby capturing all 
network packets regardless of their intended recipient, was a trivial and highly effective method for an attacker to harvest user 
login credentials. This method provided no recourse for end-users seeking to protect their passwords while transmitting data over 
the network. The subsequent and necessary shift away from cleartext communication was driven by the critical need for security on 
the World Wide Web.

Netscape Communications pioneered Secure Sockets Layer (SSL), releasing the first public version, SSL 2.0, in 1995. Recognizing 
persistent security weaknesses in the protocol's later iterations, the Internet Engineering Task Force (IETF) developed Transport 
Layer Security (TLS), releasing TLS 1.0 in 1999 as the direct, more secure successor to SSL 3.0. Further development led to the 
significant overhaul with TLS 1.3 in 2018. TLS operates at the OSI model's transport layer as a cryptographic protocol designed 
to ensure **confidentiality** and **integrity** of data exchanged between a client and a server over insecure mediums. Without 
these guarantees, applications such as online banking, commerce, and secure messaging would be unusable in a networked environment. 

The adoption of TLS has extended its security properties across numerous application protocols, conventionally denoted by appending 
an "S" for Secure to the protocol name. Examples include HTTP becoming HTTPS, DNS evolving into DNS over TLS (DoT), MQTT becoming 
MQTTS, and SIP becoming SIPS. The underlying security relies on the use of digital certificates. A party requiring identification, 
typically a server, must first acquire a signed TLS certificate. This process involves the server administrator generating a 
Certificate Signing Request (CSR), which is submitted to a trusted Certificate Authority (CA). The CA validates the request and 
issues the digital certificate. The validity of this certificate and the identity of the server can then be confirmed by clients 
through the verified signatures, provided the root certificates of the issuing CA are locally installed and trusted on the client 
host. While CAs generally charge an annual fee for issuance, services like Let's Encrypt offer free certificates. Conversely, the 
use of a self-signed certificate cannot establish authenticity, as no trusted third party has confirmed the server's identity.

---

| Description | Code/Command |
| :--- | :--- |
- Protocols secured by the addition of SSL/TLS | HTTP, DNS, MQTT, SIP |
- Resulting Secure Protocol Names | HTTPS, DoT, MQTTS, SIPS |
- Request sent to a CA to obtain a signed certificate | Certificate Signing Request (CSR) |
- Protocol room to check for TLS handshake details | Network Security Protocols room |

---

### Key Takeaways

* Early network communication methods were highly vulnerable to simple packet-capturing using network cards set to promiscuous mode, allowing cleartext credential interception.
* TLS (Transport Layer Security) is the modern cryptographic protocol standard, superseding all versions of the deprecated SSL (Secure Sockets Layer).
* TLS provides secure communication by guaranteeing both data confidentiality (preventing unauthorized reading) and data integrity (preventing undetected modification).
* Server authentication requires the server to obtain a signed TLS certificate from a trusted Certificate Authority (CA) after submitting a Certificate Signing Request (CSR).
* A self-signed certificate, lacking CA validation, cannot provide external proof of a server's authenticity and should be viewed with skepticism in public deployments.
