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

* Early network communication methods were highly vulnerable to simple packet-capturing using network cards set to promiscuous mode,
  allowing cleartext credential interception.
* TLS (Transport Layer Security) is the modern cryptographic protocol standard, superseding all versions of the deprecated SSL
  (Secure Sockets Layer).
* TLS provides secure communication by guaranteeing both data confidentiality (preventing unauthorized reading) and data integrity
  (preventing undetected modification).
* Server authentication requires the server to obtain a signed TLS certificate from a trusted Certificate Authority (CA) after
  submitting a Certificate Signing Request (CSR).
* A self-signed certificate, lacking CA validation, cannot provide external proof of a server's authenticity and should be viewed
  with skepticism in public deployments.

---
## HTTPS

---
## The Hypertext Transfer Protocol (HTTP) 
traditionally relies on TCP, defaulting to port 80, and transmits all data in cleartext. This fundamental lack of confidentiality makes
traffic interception trivial; an adversary can easily monitor and read all client-server communications, including sensitive data. 
Establishing an HTTP session requires a client to first resolve the domain name to an IP address, then execute a standard TCP 
three-way handshake with the server, and finally, issue HTTP requests, such as `GET / HTTP/1.1`. The initial handshake precedes the 
application-layer HTTP communication, which is subsequently followed by the TCP connection termination packets.

---
## HTTPS, or Hypertext Transfer Protocol Secure, 
fundamentally operates as HTTP over TLS (Transport Layer Security). This overlay 
introduces a mandatory security layer between the TCP and HTTP layers. To establish a secure HTTPS connection (after DNS resolution), 
three distinct steps are necessary: first, the standard TCP three-way handshake must be completed; second, the TLS session negotiation 
and establishment must occur; and third, the client and server communicate using the application-layer HTTP protocol, issuing requests 
like `GET / HTTP/1.1`. . During packet capture analysis, the TLS negotiation phase involves the exchange of several packets used to 
agree upon cryptographic parameters. Once established, the HTTP application data is exchanged, but it is classified merely as 
"Application Data" by the packet capture tool because the encryption prevents the protocol analyzer from confirming it is specifically 
HTTP traffic running over the standard port 443.

All data packets exchanged over HTTPS are encrypted, rendering the captured data as indecipherable gibberish without the requisite 
decryption key. Attempting to follow the packet stream yields only unintelligible content. The only method to view the plaintext 
HTTP traffic is by gaining access to the session's encryption key. While this is highly improbable in a real-world scenario, packet 
analyzers like Wireshark can decrypt the stream if the private key used for encryption is supplied. When decrypted, the traffic is 
revealed to be regular HTTP communication, demonstrating that TLS provides confidentiality without necessitating any modifications 
to the lower-layer (TCP/IP) or higher-layer (HTTP) protocols. TLS successfully integrated security by acting as an intermediary layer.

---

| Description | Code/Command |
| :--- | :--- |
| Example HTTP request | `GET / HTTP/1.1` |
| Default cleartext HTTP port | `80` |
| Default secure HTTPS port | `443` |

---

### Key Takeaways

* HTTP transmission is inherently insecure, relying on cleartext, making all traffic easily readable by passive network interception.
* HTTPS operates as HTTP layered over TLS, securing communications without changing the core TCP/IP or HTTP protocols.
* HTTPS establishment requires an intermediate step—the TLS negotiation and session establishment—between the TCP handshake and
  application-layer data exchange.
* TLS encrypts all traffic, resulting in packet capture tools identifying data only as "Application Data."
* Decrypting HTTPS traffic requires access to the session's private encryption key, confirming that the underlying communication
  remains standard HTTP.




