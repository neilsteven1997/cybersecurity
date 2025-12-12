# Networking Core Protocols 

---
## DNS: Remembering Addresses

The Domain Name System (DNS) is an indispensable Application Layer (OSI Layer 7) protocol responsible for translating human-readable 
domain names into corresponding Internet Protocol (IP) addresses, eliminating the need for users to memorize numerical network 
locations. Standard DNS traffic primarily leverages User Datagram Protocol (UDP) on port 53 for speed in typical queries, falling 
back to Transmission Control Protocol (TCP) on port 53 primarily for zone transfers or when the response data exceeds the initial 
512-byte limit. This underlying mechanism supports essential internet functions, utilizing a hierarchy of defined resource records.

Fundamental DNS resolution relies on specific record types. 
- The Address (A) record maps a hostname to an IPv4 address,
- while the Quad-A (AAAA) record performs the equivalent mapping for an IPv6 address.
- Conversely, the Canonical Name (CNAME) record functions as an alias, directing one domain name (e.g., a subdomain) to another domain
  name, often to simplify management or point services to a canonical host.
- Separately, the Mail Exchange (MX) record is crucial for email routing, specifying the authoritative mail server 
designated to process emails for a given domain.

When a client initiates a request, such as navigating a web browser, the resolution process involves querying the DNS server for 
the appropriate A and AAAA records. This is distinct from an email server operation, which specifically queries the DNS system for 
the MX record to determine the mail delivery endpoint. Network diagnostics, utilizing tools like `nslookup` or packet analyzers 
suchers as `tshark`, allow for command-line observation of this exchange, revealing the sequential or simultaneous transmission 
of DNS query packets for both IPv4 and IPv6 records and the subsequent non-authoritative responses containing the resolved addresses.

---
# WHOIS

---
## Domain Name System (DNS) resolution, 
which maps a domain name to its corresponding IP address via records such as A, AAAA, and MX, 
is predicated upon the authority granted to the domain registrant. The individual or entity that registers a domain name acquires 
the exclusive power to define these critical DNS records. Domain registration requires payment of an annual fee and mandates the 
provision of accurate contact information for the registrant.

This contact information constitutes the WHOIS record, a publicly accessible database detailing the domain ownership.  Although 
"`WHOIS`" is not an acronym, this record serves as a fundamental resource for identifying the entity responsible for a domain. 
WHOIS records typically contain the registrant's name, physical address, email, and phone number, along with key administrative 
and technical data points, including the domain's creation, update, and expiration dates, as well as the registering authority 
(Registrar) details.

To mitigate privacy concerns arising from the public nature of WHOIS data, registrants frequently utilize privacy services. These 
services mask the individual's or organization's details, substituting them with the proxy service's information, such as 
"Registration Private" and the service's organizational details, effectively shielding the true registrant's identity from public 
WHOIS lookups. This record information can be queried directly via command-line tools like `whois` or through various online services.

>[!Tip]
>`whois x.com`
>`whois twitter.com`

---
# HTTP(S): Accessing the Web

The fundamental protocols governing web browser communication with web servers are the Hypertext Transfer Protocol (HTTP) and its 
secure counterpart, HTTPS, which adds a layer of encryption. Both protocols rely on the Transmission Control Protocol (TCP) for 
reliable data transmission. HTTP typically operates on TCP port 80, while HTTPS uses TCP port 443; alternative ports such as 8080 
and 8443 are also sometimes utilized. HTTP defines several methods, or commands, used by the client to interact with the server. 

### Key methods include 
- **GET**, which is used to request and retrieve a specified resource like an HTML file or an image, 
- and **POST**, which facilitates submitting data from the client to the server, such as form contents or file uploads. Less commonly,
- **PUT** is employed to create a new resource or update an existing one on the server, 
- while **DELETE** is used to remove a designated resource.

Behind the surface-level rendering of a webpage, the HTTP exchange involves significant metadata transmission not displayed to the 
user. Packet analysis tools like Wireshark reveal the full exchange, demonstrating the explicit request headers sent by the client 
and the detailed response headers from the server, which may contain information such as the server software version and the last 
modification date of the requested page.

Direct interaction with a web server for diagnostic purposes can be achieved using command-line tools such as `telnet`. This allows 
an operator to manually "speak HTTP" by issuing standard protocol commands, which is highly efficient for troubleshooting. 
For example, retrieving the default page requires transmitting a minimum of `GET / HTTP/1.1` followed by a `Host: [hostname]` header, 
though the required components can vary based on the specific server configuration. This method can be adapted to request any specific 
file by modifying the GET line, such as `GET /file.html HTTP/1.1`. 

Use telnet to access the file flag.html on 10.49.146.142. What is the hidden flag?

### Make sure you are using the correct syntax:

1. Open your terminal and type `telnet 10.49.146.142 80`.
2. After connecting, type `GET /flag.html HTTP/1.1` and press Enter.
3. Then type `Host: 10.49.146.142` and press Enter again.
4. Finally, press Enter once more to send the request.

`THM{TELNET-HTTP}`

---
## FTP: Transferring Files 

## The File Transfer Protocol (FTP) 
is a specialized application layer protocol designed specifically for efficient file transfer, 
often achieving higher data throughput speeds than general-purpose protocols like HTTP under comparable conditions. FTP is 
fundamentally command-based, utilizing specific instructions to manage client-server interaction. Essential commands include 
- **USER** and **PASS** for authentication, 
- **RETR** (retrieve) for downloading a file from the server, 
- and **STOR** (store) for uploading a file to the server.

The server operates by listening on TCP port `21`, which is designated for the **control connection**. This control channel manages 
commands, authentication, and responses. Critically, the actual file transfer and directory listings occur over **separate data 
connections** established from the client back to the server. This dual-channel architecture is a defining characteristic of FTP.

An FTP client session illustrates this process. After initiating a connection, the client issues the **USER** command 
(e.g., `anonymous`) and the **PASS** command (often an empty or placeholder email address for anonymous access). Upon successful 
authentication, a command like `ls` is translated by the client into the protocol command **LIST** sent over the control channel. 
The response to **LIST** and file retrieval initiated by **RETR** both trigger the establishment of a new, ephemeral data connection 
to transfer the listing or the file contents (e.g., `coffee.txt`). Observing the transaction via packet analysis reveals the 
distinction between the command traffic on the control port and the data traffic occurring on these separate, dynamically 
established ports .







