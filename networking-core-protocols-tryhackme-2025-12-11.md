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
- Conversely, the Canonical Name (CNAME) record functions as an alias, directing one domain name (e.g., a subdomain) to another domain name, often to simplify management or point services to a canonical host.
- Separately, the Mail Exchange (MX) record is crucial for email routing, specifying the authoritative mail server 
designated to process emails for a given domain.

When a client initiates a request, such as navigating a web browser, the resolution process involves querying the DNS server for 
the appropriate A and AAAA records. This is distinct from an email server operation, which specifically queries the DNS system for 
the MX record to determine the mail delivery endpoint. Network diagnostics, utilizing tools like `nslookup` or packet analyzers 
suchers as `tshark`, allow for command-line observation of this exchange, revealing the sequential or simultaneous transmission 
of DNS query packets for both IPv4 and IPv6 records and the subsequent non-authoritative responses containing the resolved addresses.

---



