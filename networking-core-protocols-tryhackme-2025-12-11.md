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
- **USER**
- and **PASS** for authentication, 
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

---
1. Client Terminal Commands
These are the commands you type into the local FTP client session:
Command,Action,Example Usage
- `ftp [IP_Address]`,Initiates the connection to the remote FTP server.,ftp 10.49.146.142
- `anonymous`,The username typically used for anonymous login when prompted.,Name (server IP): anonymous
- `ls` (or dir),Lists the files and directories available on the server.,ftp> ls
- `type ascii`,Switches the transfer mode to ASCII for text files.,ftp> type ascii
- `type binary`,"Switches the transfer mode to Binary for non-text files (executables, images, etc.).",ftp> type binary
- `get [filename]`,Downloads the specified file from the server to your local machine.,ftp> get coffee.txt
- `put [filename]`,Uploads the specified local file to the remote server.,ftp> put my_file.txt
- `quit` (or bye),Disconnects the client from the FTP server.,ftp> quit

2. Underlying FTP Protocol Commands
These are the actual commands defined by the FTP protocol, sent over the control connection (TCP Port 21) from the client to 
the server:
Protocol Command,Description,Client Command Equivalent
- `USER`,Sends the username for authentication.,(Implicitly sent after entering username)
- `PASS`,Sends the password for authentication.,(Implicitly sent after entering password)
- `LIST`,Requests a listing of files and directories.,ls or dir
- `RETR`,Requests to download (retrieve) a specified file.,get
- `STOR`,Requests to upload (store) a file to the server.,put
- `TYPE A`,Sets the data transfer type to ASCII.,type ascii
- `TYPE I`,Sets the data transfer type to Image (Binary).,type binary

Example Session Flow
The terminal output provided demonstrates the sequence:
1. Connection: user@TryHackMe$ `ftp 10.49.146.142`
2. Login: Enter `anonymous` (sends USER) followed by the password (sends PASS).
3. Directory Listing: ftp> `ls` (sends LIST)
4. Mode Change: ftp> `type ascii` (sends TYPE A)
5. Download: ftp> `get coffee.txt` (sends RETR coffee.txt)
6. Disconnection: ftp> `quit`

Using the FTP client ftp on the AttackBox, access the FTP server at 10.49.144.88 and retrieve flag.txt. What is the flag found?
`THM{FAST-FTP}`

---
## Simple Mail Transfer Protocol (SMTP) 
defines the mechanism for email transmission between a mail client and a mail server, and critically, between two mail servers. 
This process is analogous to physical mail dispatch and utilizes a series of defined, text-based commands over a reliable connection, 
typically TCP port 25, the default listening port for SMTP servers.

1. The SMTP session initiation begins with the client issuing either the **HELO** or **EHLO** (Extended HELO) command to identify 
itself to the server. 

2. Following successful initiation, the client must specify the mail origin using the **MAIL FROM** command, which provides the 
sender's email address. 

3. The next step is defining the destination via the **RCPT TO** command, specifying the recipient's email address.

4. Once the source and destination are defined, the client uses the **DATA** command to signal that the full body and headers of the 
email message will follow. The server responds, preparing to receive the content. 

5. The client then transmits the complete email, 
concluding the message with a single period (`.`) on a line by itself, which serves as the end-of-message delimiter.  This direct 
protocol interaction, which can be manually replicated using a `telnet` client, reveals the fundamental command exchange hidden 
beneath modern email client interfaces, providing insight into the structure of text-based protocols like POP3 and IMAP.

---
## The Post Office Protocol version 3 (POP3) 
is the fundamental application layer protocol utilized by mail clients to communicate with a mail server for the primary purpose 
of retrieving email messages. Conceptually, while the Simple Mail Transfer Protocol (SMTP) facilitates the sending of email, 
POP3 manages the client-side retrieval, comparable to a user checking their physical mailbox. The POP3 server listens on 
TCP port 110 by default for client connections.

A POP3 session commences with client authentication, using the **USER** command to identify the account followed immediately by 
the **PASS** command to provide the corresponding password. After successful login, the client can issue informational requests, 
such as **STAT**, which returns the number of messages and their cumulative size. The **LIST** command provides a detailed 
enumeration of all messages along with their individual sizes. 

Retrieving a specific message is accomplished using the **RETR** command, specifying the message number. After retrieval, the 
client can use the **DELE** command to mark a message for deletion on the server. The session is concluded with the **QUIT** 
command, which executes any pending changes, such as deletions, before closing the connection. As the interaction, including the 
transmission of the username and password, is conducted in cleartext, network packet interception makes the exchanged data, 
including authentication credentials, entirely readable to an attacker. This inherent lack of encryption necessitates the use 
of secure alternatives like POP3S or the Internet Message Access Protocol (IMAP) for modern security standards.

Common POP3 commands are:
- `USER <username>` identifies the user
- `PASS <password>` provides the user’s password
- `STAT` requests the number of messages and total size
- `LIST` lists all messages and their sizes
- `RETR <message_number>` retrieves the specified message
- `DELE <message_number>` marks a message for deletion
- `QUIT` ends the POP3 session applying changes, such as deletions

Here are the steps to connect to the POP3 server and retrieve emails using telnet:
1. Open your terminal and type the command: `telnet 10.49.144.88 110`
2. Once connected, you should see a welcome message.
3. Type `AUTH` and `press Enter`, then type `PLAIN` and `press Enter` to select the authentication method.
4. Next, type `USER strategos` and `press Enter`.
5. Then, type `PASS followed by your password` and `press Enter` to log in.
6. Type `STAT` to check the number of messages and their size.
7. Use `LIST` to see all messages.
8. To retrieve a specific message, use `RETR 3` (or replace '3' with the desired message number).
9. Finally, type `QUIT` to log out.

Looking at the traffic exchange, what is the name of the POP3 server running on the remote server?
`Dovecot`

Use telnet to connect to 10.49.178.253’s POP3 server. What is the flag contained in the fourth message?
`THM{TELNET_RETR_EMAIL}`

---















