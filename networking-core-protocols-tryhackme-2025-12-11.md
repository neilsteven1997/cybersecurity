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


