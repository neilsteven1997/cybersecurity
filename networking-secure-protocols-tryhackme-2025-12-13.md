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

How many packets did the TLS negotiation and establishment take in the Wireshark HTTPS screenshots above?
- 8

What is the number of the packet that contain the GET /login when accessing the website over HTTPS?
- 10

---
## SMTPS, POP3S, and IMAPS

The methodology for securing standard cleartext communication protocols such as SMTP, POP3, and IMAP directly mirrors the 
transition from HTTP to HTTPS. Each protocol is appended with an "S" to denote its secure variant: SMTP becomes SMTPS, POP3 
becomes POP3S, and IMAP becomes IMAPS. Functionally, deploying these email protocols over TLS (Transport Layer Security) 
follows the exact process established for HTTPS, where the TLS handshake is negotiated directly after the TCP connection to 
encrypt the application data layer. 

Unsecured email protocols historically utilized distinct default TCP ports: SMTP operated on port 25, POP3 on port 110, and 
IMAP on port 143. The secure, TLS-enabled counterparts utilize different port assignments to signify the encryption requirement: 
HTTPS uses 443; SMTPS uses ports 465 and 587; POP3S uses 995; and IMAPS uses 993. The rationale for applying TLS to these and 
many other protocols is consistent: to enforce confidentiality and integrity across the network session, effectively mitigating 
the threat of passive packet interception and cleartext data exposure.

| Description | Insecure Port | Secure Port |
| :--- | :--- | :--- |
| HTTP | 80 | 443 (HTTPS) |
| SMTP | 25 | 465 and 587 (SMTPS) |
| POP3 | 110 | 995 (POP3S) |
| IMAP | 143 | 993 (IMAPS) |

---

### Key Takeaways

* Securing email protocols (SMTP, POP3, IMAP) involves overlaying them with TLS, resulting in SMTPS, POP3S, and IMAPS, respectively.
* The security mechanism is identical to HTTPS, requiring a TLS handshake post-TCP connection to initiate encryption.
* Secure protocols are generally assigned separate default TCP port numbers from their cleartext counterparts to enforce encryption.

If you capture network traffic, in which of the following protocols can you extract login credentials: SMTPS, POP3S, or IMAP?
- IMAP
  
---
## SSH

The historical use of the TELNET protocol for remote system administration posed a critical security risk, transmitting all 
command and login data in cleartext. Any network monitor could trivially capture and compromise user credentials. This vulnerability 
mandated the development of a secure alternative, leading Tatu Ylönen to create the **Secure Shell (SSH)** protocol, with the 
first version, SSH-1, released as freeware in 1995. A more robust and secure revision, SSH-2, was defined shortly thereafter in 
1996. The open-source community's adoption was solidified in 1999 when the OpenBSD developers released **OpenSSH**, which now 
forms the basis for most contemporary SSH client implementations.

OpenSSH provides several core security and utility benefits that fundamentally address the deficiencies of TELNET. 

Key capabilities include robust **security authentication** methods, supporting public key and two-factor authentication 
alongside traditional password access. **Confidentiality** is enforced via end-to-end encryption, effectively neutralizing 
eavesdropping attempts, and the protocol incorporates mechanisms to detect changes in server keys, which helps protect 
against man-in-the-middle attacks. Furthermore, cryptography ensures the **integrity** of exchanged data, protecting against 
undetected modification. Beyond core remote access, SSH supports **tunneling**, allowing other protocols to be routed securely 
through an SSH session, effectively creating a VPN-like connection. The protocol also facilitates **X11 Forwarding**, enabling 
the remote use of graphical applications on a Unix-like system. While the insecure TELNET server listens on TCP port 23, the 
secure SSH server defaults to listening on **TCP port 22**. The command structure for initiating a connection is straightforward, 
typically requiring the syntax `ssh username@hostname`, or simply `ssh hostname` if the local and remote usernames are 
identical. Authentication can be password-based or instantaneous if public-key pairs are configured. Using the `-X` argument 
during connection initiation, such as `ssh 192.168.124.148 -X`, is required to enable remote graphical interface forwarding, 
provided the local system supports the necessary graphical environment.

| Description | Code/Command |
| :--- | :--- |
| Basic SSH connection syntax | `ssh username@hostname` |
| Connection syntax using default local username | `ssh hostname` |
| SSH connection with X11 forwarding enabled | `ssh 192.168.124.148 -X` |
| Argument required for graphical interface support | `-X` |

---

### Key Takeaways

* TELNET's cleartext transmission is a critical risk, allowing trivial capture of login credentials.
* SSH, and specifically OpenSSH, resolves this by implementing end-to-end encryption, ensuring confidentiality and integrity.
* SSH supports multiple secure authentication methods, including public key and two-factor authentication.
* SSH provides advanced features such as secure tunneling for other protocols and X11 Forwarding for remote graphical application use.
* SSH servers default to TCP port 22, whereas the deprecated TELNET uses port 23.

What is the name of the open-source implementation of the SSH protocol?
- OpenSSH

---
## SFTP and FTPS

The **SSH File Transfer Protocol (SFTP)**, a component of the overarching SSH protocol suite, provides secure file transfer 
capabilities and operates on the same default TCP port, 22. When enabled within the OpenSSH server configuration, clients can 
initiate a connection using the `sftp username@hostname` command.  Once authenticated, the protocol uses a command structure 
similar to Unix shell commands for operations, such as `get filename` for downloading and `put filename` for uploading.

SFTP must be differentiated from **FTPS (File Transfer Protocol Secure)**. FTPS secures standard FTP by incorporating the TLS 
protocol, mirroring the methodology used for HTTPS. While FTP utilizes default port 21, FTPS typically operates on port 990. 
A key functional difference is that FTPS establishment is more complex: it mandates a proper TLS certificate setup and often 
poses difficulties for network administrators as it requires separate control and data connections, making it challenging to 
allow through strict firewall rules.

In contrast, setting up an SFTP server is straightforward, requiring only the activation of the relevant option within the 
existing OpenSSH server configuration. Both FTPS and other secure protocols like HTTPS, SMTPS, POP3S, and IMAPS rely on the 
robust security mechanisms provided by TLS, necessitating a valid, configured TLS certificate for secure operation.

| Description | Code/Command |
| :--- | :--- |
| SFTP connection command syntax | `sftp username@hostname` |
| Command to download a file via SFTP | `get filename` |
| Command to upload a file via SFTP | `put filename` |
| Default cleartext FTP port | `21` |
| Default secure FTPS port | `990` |

---

### Key Takeaways

* SFTP is part of the SSH suite, uses port 22, and is simpler to configure by enabling an OpenSSH server option.
* FTPS is secured using TLS, uses port 990, and requires a dedicated TLS certificate setup.
* FTPS is structurally more complex than SFTP due to its use of separate control and data connections, which complicates firewall
* policy configuration.
* Both SFTP and FTPS provide cryptographic protection over the network, achieving confidentiality and integrity during file transfers.
  
Click on the View Site button to access the related site. Please follow the instructions on the site to obtain the flag.
- `THM{Protocols_secur3d}`
