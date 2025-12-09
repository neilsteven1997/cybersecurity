## The OSI (Open Systems Interconnection) model is a conceptual model developed by the International Organization for 
Standardization (ISO) that describes how communications should occur in a computer network. In other words, the OSI model 
defines a framework for computer network communications. Although this model is theoretical, it is vital to learn and 
understand as it helps grasp networking concepts on a deeper level. The OSI model is composed of seven layers:

1. Physical Layer - deals with the physical connection between devices; this includes the medium, such as a wire, and the 
definition of the binary digits 0 and 1. Data transmission can be via an electrical, optical, or wireless signal. 
Consequently, we need data cables or antennas, depending on our physical medium.

2. Data Link Layer - represents the protocol that enables data transfer between nodes on the same network segment. The data 
link layer describes an agreement between the different systems on the same network segment on how to communicate. 
MAC stands for Media Access Control. They are usually expressed in hexadecimal format with a colon separating each two 
hexadecimal digits (one byte). The three leftmost bytes identify the vendor.
- MAC Address: a4:c3:f0:85:ac:2d
- Vendor: a4:c3:f0 (Intel)
- Unique address of the network interface: 85:ac:2d

3. Network Layer - The data link layer focuses on sending data between two nodes on the same network segment. The network 
layer handles logical addressing and routing, i.e., finding a path to transfer the network packets between the diverse 
networks. The network layer will route the network packets through the path it deems better.
- Examples of the network layer include Internet Protocol (IP), Internet Control Message Protocol (ICMP), and Virtual 
Private Network (VPN) protocols such as IPSec and SSL/TLS VPN.

4. Transport Layer - enables end-to-end communication between running applications on different hosts, which can support 
various functions like flow control, segmentation, and error correction.
- Example: Your browser is connected to TryHackMe server.
- Examples of layer 4 are Transmission Control Protocol (TCP) and User Datagram Protocol (UDP).

5. Session Layer - is responsible for establishing, maintaining, and synchronising communication between applications 
running on different hosts. Establishing a session means initiating communication between applications and negotiating 
the necessary parameters for the session. Data synchronisation ensures that data is transmitted in the correct order 
and provides mechanisms for recovery in case of transmission failures.
- Examples of the session layer are Network File System (NFS) and Remote Procedure Call (RPC).

6. Presentation Layer - ensures the data is delivered in a form the application layer can understand. Layer 6 handles
data encoding, compression, and encryption. An example of encoding is character encoding, such as ASCII or Unicode.

Various standards are used at the presentation layer. Consider the scenario where we want to send an image via email. 
First, we use JPEG, GIF, and PNG to save our images; furthermore, although hidden from the user by the email client, 
we use MIME (Multipurpose Internet Mail Extensions) to attach the file to our email. MIME encodes a binary file using 
7-bit ASCII characters.

7. Application Layer - The application layer provides network services directly to end-user applications. Your web browser 
would use the HTTP protocol to request a file, submit a form, or upload a file.
- Examples of Layer 7 protocols are HTTP, FTP, DNS, POP3, SMTP, and IMAP.

> [!Tip]
> The numbering starts with the physical layer being layer 1, while the top layer, the application layer, is layer 7. 
To help you remember the layers from bottom to top, you can use a mnemonic such as “Please Do Not Throw Spinach Pizza Away.”

---
## Summarized ISO OSI layers:
- Layer 7 Application layer Providing services and interfaces to applications HTTP, FTP, DNS, POP3, SMTP, IMAP

- Layer 6     Presentation layer     Data encoding, encryption, and compression     Unicode, MIME, JPEG, PNG, MPEG

- Layer 5     Session layer     Establishing, maintaining, and synchronizing sessions     NFS, RPC

- Layer 4     Transport layer     End-to-end communication and data segmentation     UDP, TCP

- Layer 3     Network layer     Logical addressing and routing between networks     IP, ICMP, IPSec

- Layer 2     Data link layer     Reliable data transfer between adjacent nodes     Ethernet (802.3), Wi-Fi (802.11)

- Layer 1     Physical layer     Physical data transmission media     Electrical, optical, and wireless signals

---
## TCP/IP Model 
- stands for Transmission Control Protocol/Internet Protocol and was developed in the 1970s by the Department of Defense
- (DoD). I hear you ask why DoD would create such a model. One of the strengths of this model is that it allows a network
- to continue to function as parts of it are out of service, for instance, due to a military attack. This capability is
- possible in part due to the design of the routing protocols to adapt as the network topology changes.

From top to bottom, we have:
- Application Layer: The OSI model application, presentation and session layers, i.e., layers 5, 6, and 7, are grouped into
  the application layer in the TCP/IP model.
- Transport Layer: This is layer 4.
- Internet Layer: This is layer 3. The OSI model’s network layer is called the Internet layer in the TCP/IP model.
- Link Layer: This is layer 2.

Showing how the TCP/IP model layers map to the ISO/OSI model layers.

Layer Number     ISO OSI Model     TCP/IP Model (RFC 1122)     Protocols
- 7     Application Layer     Application Layer     HTTP, HTTPS, FTP, POP3, SMTP, IMAP, Telnet, SSH,
- 6     Presentation Layer 
- 5     Session Layer
- 4     Transport Layer     Transport Layer     TCP, UDP
- 3     Network Layer     Internet Layer     IP, ICMP, IPSec
- 2     Data Link Layer     Link Layer     Ethernet 802.3, WiFi 802.11
- 1     Physical Layer

Many modern networking textbooks show the TCP/IP model as five layers instead of four. For example, in Computer Networking: 
A Top-Down Approach 8th Edition, Kurose and Ross describe the following five-layer Internet protocol stack by including 
the physical layer:
- Application
- Transport
- Network
- Link
- Physical

---
#  IP Addresses And Subnets

`192.168.1.0` is the network address
`192.168.1.255` is the broadcast address. Sending to the broadcast address targets all the hosts on the network. 
With simple math, you can conclude that we cannot have more than 4 billion unique IPv4 addresses. If you are curious 
about the math, it is approximately 232 because we have 32 bits. This number is approximate because we didn’t consider 
network and broadcast addresses.

---
## Looking Up Your Network Configuration

- Microsoft Windows: Use the command `ipconfig.`
- Linux/UNIX-based Systems (and older Linux): Use the command `ifconfig`.
- Modern Linux Systems: Use the preferred command ip address show, which can be shortened to `ip a s`.

- For a private IP address to access the Internet, the router must have a public IP address and must support Network Address
  Translation (NAT)

- The Host IP Address is the unique identifier assigned to a single device, allowing for direct, one-to-one communication
  (unicast). 

- The Subnet Mask, however, is not a communication address; its purpose is purely to define the network boundaries by
  separating the Host IP into two parts: the network portion and the host portion. Routers use the Subnet Mask to
  determine if a destination is local or remote. 

Finally, the Broadcast IP Address is a special address used for one-to-all communication (broadcast) within the local 
subnet; any packet sent to this address will be received by every single device on that specific network segment.

### Two types of IP addresses:
- Public IP addresses
- Private IP addresses
RFC 1918 defines the following three ranges of private IP addresses:
- `10.0.0.0 - 10.255.255.255 (10/8)`
- `172.16.0.0 - 172.31.255.255 (172.16/12)`
- `192.168.0.0 - 192.168.255.255 (192.168/16)`

---
## Routing
A router forwards data packets to the proper network. Usually, a data packet passes through multiple routers before it 
reaches its final destination. The router functions at layer 3, inspecting the IP address and forwarding the packet to 
the best network (router) so the packet gets closer to its destination.

## UDP And TCP
The IP protocol gets us to the correct machine (the host, identified by the IP address). However, to enable applications 
(processes) on those hosts to communicate, we need transport protocols. The two options are UDP and TCP, both operating 
at Layer 4 of the network stack.

## UDP (User Datagram Protocol)
UDP is a simple, connectionless protocol. It allows me to target a specific process on a host using a port number. Port 
numbers use two octets, giving a range of 1 to 65535 (since port 0 is reserved).
- Key Feature: Connectionless. It requires no connection setup.
- Reliability: UDP offers no guarantee of delivery; it doesn't even have a mechanism to confirm the packet was received.
- Analogy: This is like sending standard mail with no delivery confirmation. The trade-off is speed—it’s faster than reliable 
protocols because it doesn't wait for acknowledgments.

## TCP (Transmission Control Protocol)
TCP is the alternative, a connection-oriented transport protocol designed for reliable data delivery between networked 
processes.
- Key Feature: Connection-oriented. It requires a dedicated connection establishment before any data transfer can begin.
- Reliability: TCP ensures reliability through sequence numbers (each data octet is numbered to identify missing or duplicate 
packets) and acknowledgment numbers (the receiver confirms the last successfully received octet).
- Connection Setup: A connection is formally established using a three-way handshake:
1. SYN: The client sends a SYN (Synchronise) packet to the server, including the client's initial sequence number.
2. SYN-ACK: The server responds with a SYN-ACK (Synchronise-Acknowledgment) packet, including its own initial sequence number.
3. ACK: The client finishes the handshake by sending an ACK (Acknowledgment) packet back to confirm the SYN-ACK reception.

## Port Numbers in TCP/UDP
Like UDP, TCP uses port numbers to identify the specific process that is either initiating a connection or listening for 
one. Since port numbers utilize two octets, the valid range for any process is between `1 and 65535`; port `0` remains reserved 
and unusable. 
- What is the approximate number of port numbers (in thousands)? 65 thousand

---
## Encapsulation and The Life of a Packet
-is the core concept that keeps network layers independent. It is the process where every layer adds its own identifying 
header (and sometimes a trailer) to the data unit received from the layer above it, then sends the whole encapsulated 
unit to the layer below. This structure allows each layer to focus strictly on its own function.

The process flows downward:
1. Application Data: The user's input (e.g., an email message) is formatted by the application protocol (e.g., HTTP) and 
sent down.
2. Transport Segment/Datagram: The transport layer (TCP or UDP) adds its header (containing port numbers) to create a TCP 
segment or UDP datagram.
3. Network Packet: The network layer (IP layer) adds an IP header (containing source and destination IP addresses) to create 
an IP packet. This is what can be routed over the internet.
4. Data Link Frame: The link layer (Ethernet or WiFi) adds a header and a trailer to the IP packet, creating a frame.

>[!Note]
>This process is strictly reversed on the receiving end (known as decapsulation) until the original application data is 
extracted and presented to the user.

---
## The Simplified Life of a Packet
When I search for a room on a site like TryHackMe, the layers immediately begin their work:

1. Application: My browser prepares the HTTP request containing the search query.

2. TCP: The TCP layer first completes a three-way handshake to establish a reliable connection with the web server. Once the 
connection is confirmed, the HTTP request is sent inside TCP segments.

3. IP: The IP layer adds the source IP (my computer) and the destination IP (the TryHackMe server) to the packet and pushes 
it to my link layer.

4. Link Layer: My laptop adds the necessary physical header/trailer (Frame) and sends it out to the router.

5. Routing: The router removes the link layer frame headers, inspects the destination IP, determines the best path, and forwards 
the packet. This hop-by-hop process repeats until the packet reaches the target network.

>[!Note]
>At the destination, the layers strip their headers one by one (decapsulation) until the server's application receives
>the original search query.

---
## Using TELNET for TCP Service Interaction

TELNET is a network protocol that facilitates a remote terminal connection, allowing you to connect to and issue text 
commands to a remote system. While historically used for remote administration, the client (telnet) can be used to 
communicate with any server listening on a TCP port.

We can demonstrate this interaction by connecting to various services running on a target virtual machine:

1. Echo Server (Port 7)
The echo server is a simple service that returns everything you send it. To connect to this service, you specify the target host and the default port 7.
- Behavior: The server immediately sends back any text input you type.
- Closing Connection: The connection remains open until you manually close it (e.g., by pressing CTRL+] in many terminal clients).

>[!Note]
>Services like Echo are considered security risks and are typically disabled in production environments.

2. Daytime Server (Port 13)
The daytime server provides a lightweight way to retrieve a time and date stamp.
- Behavior: Upon connection to port 13, the server immediately replies with the current day and time and then automatically closes the connection. This shows a complete, single-transaction communication.

3. Web (HTTP) Server (Port 80)
You can manually send HTTP requests to a web server using `telnet`. This illustrates how a browser fundamentally communicates with a server.
1.Connect: Initiate the connection to the target host on TCP port 80.
2.Request: You must manually type the required HTTP request lines, including the specific page you want (`GET / HTTP/1.1`)
   and the required Host header (`Host: telnet.thm`).
3.Execute: The request is finalized and sent by ensuring the last input line is a blank line (by pressing Enter twice).
4.Response: The server processes the request and returns the requested web page content (redacted in your example), along
   with the necessary HTTP status codes and headers.

Service | TCP Port | Command Sequence | Purpose 

- Echo Server
7
`telnet <target_VM_IP> 7`
Establishes connection to the echo service.

- Daytime Server
13
`telnet <target_VM_IP> 13`
Connects and retrieves the time/date before automatically closing.

- Web Server (HTTP)
80
1. `telnet <target_VM_IP> 80` 
2. `GET / HTTP/1.1 `
3. `Host: telnet.thm` 
4. Press Enter (Blank Line)
Manually sends an HTTP request to retrieve the main page.














