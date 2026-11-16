# Wireshark: Packet Operations

---

Wireshark packet analysis involves examining network traffic captures, reviewing protocol statistics, and applying filters to isolate 
events of interest. This is the second room in the Wireshark room series, following Wireshark: The Basics. The exercises use a virtual
machine (VM) containing Wireshark and several packet captures. The primary exercise file is `Exercise.pcapng`, located on the desktop.
The lab explicitly instructs analysts not to interact directly with the domains or IP addresses found in the capture.

The investigation begins with the Statistics menu, which provides an overview of captured traffic, protocols, endpoints, conversations,
and protocol-specific information. The Summary view helps establish the overall scope of a capture and develop an initial investigation
hypothesis. Resolved Addresses identifies IP addresses and hostnames obtained from DNS answers within the capture. Protocol Hierarchy 
presents protocols in a tree structure, including packet counts and percentages, allowing traffic to be examined by protocol and 
service. Individual entries can be right-clicked to filter relevant traffic.

Conversations identifies communication between two endpoints across Ethernet, IPv4, IPv6, TCP, and UDP. Endpoints provides a 
corresponding view of unique individual endpoints rather than communication pairs. Wireshark can also resolve MAC addresses into 
manufacturer names using the first three bytes of an address and IEEE-assigned manufacturer information. IP and port name resolution
can be enabled through Edit → Preferences → Name Resolution. These settings allow resolved names to appear in packet lists, 
Conversations, and Endpoints.

IP geolocation is another available analysis feature. Wireshark supports MaxMind databases, but the analyst must obtain the required 
database files and configure their location under Edit → Preferences → Name Resolution → MaxMind database directories. Once configured,
available geographic information appears in the IP protocol details for matching addresses. Internet access is required to display the 
GeoIP map; the provided lab machine does not have an active internet connection.

The Protocol Details statistics provide additional visibility into IPv4, IPv6, DNS, and HTTP traffic. IPv4 and IPv6 statistics separate
packet information according to IP version. DNS statistics organize captured DNS traffic by response codes, operation codes, classes,
query types, services, and query statistics. HTTP statistics provide an overview of HTTP requests, response codes, and original 
requests.

Wireshark provides two distinct filtering mechanisms. Capture filters determine which traffic is saved and must be configured before
capturing begins. They cannot be modified during an active capture. Display filters, by contrast, reduce the packets visible during
analysis and can be changed while capturing. Their expressions are not interchangeable. Capturing all traffic and applying display 
filters during investigation is the workflow described in the material, although capture filters require careful preparation because 
an incorrect expression may exclude the traffic being investigated.

Capture filter syntax uses byte offsets, hexadecimal values, masks, and Boolean operators. Its components include scope, direction,
and protocol. Scope can specify a host, network, port, or port range. Direction can identify source, destination, either endpoint, 
or both endpoints. Supported protocol identifiers include Ethernet, wireless LAN, IPv4, IPv6, ARP, RARP, TCP, and UDP.

Display filters support a much broader range of protocol fields and are used for detailed packet-level searches. The material 
identifies support for approximately 3,000 protocols. Wireshark's Display Filter Expression interface helps analysts identify 
available protocol fields, accepted value types, and predefined values. This is particularly useful when the required field or 
expression is unfamiliar. Display filters can also be combined with Coloring Rules to visually distinguish relevant packets.

Comparison operators support equality, inequality, greater-than, less-than, and their inclusive equivalents. Both decimal and 
hexadecimal values can be used. Logical expressions provide AND, OR, and NOT operations for combining conditions. The source 
recommends using the negation form rather than the deprecated inequality-value syntax where consistent results are required.

The filter toolbar assists with creating and applying display filters through autocomplete and syntax validation. Filters are written 
in lowercase. Its color indicators distinguish valid expressions in green, invalid expressions in red, and warning expressions in 
yellow. Yellow indicates that a filter may work but should be replaced with a reliable expression.

Protocol-specific filtering provides more precise control over network and application traffic. IP filters examine network-layer 
information, including addresses, IP versions, time-to-live values, service types, flags, and checksums. The `ip.addr` field matches
traffic involving an address without considering direction, whereas `ip.src` and `ip.dst` distinguish traffic according to its source 
or destination.

TCP and UDP filters examine transport-layer information, including ports, sequence numbers, acknowledgement numbers, window sizes, 
timestamps, flags, packet lengths, and protocol errors. Application-layer filters provide access to protocol-specific information such
as HTTP requests and responses and DNS queries and responses. HTTP filters can isolate response codes and request methods, while DNS
filters can distinguish queries, responses, and record types.

The Display Filter Expression interface provides a reference for protocol fields and their accepted values. Since individual 
protocols expose different fields and value types, practical familiarity develops through repeated use rather than memorizing every 
available expression.

Advanced filtering extends these operations through comparison operators, set membership, and string functions. The `contains` operator
searches for a case-sensitive value within a packet field. The `matches` operator uses regular expressions to identify patterns and 
is case-insensitive. The `in` operator checks whether a field belongs to a specified collection of values.

Wireshark also provides `upper` and `lower` functions to convert string fields into uppercase or lowercase before searching. The 
`string` function converts non-string values into strings, allowing regular expressions to examine numerical fields such as frame 
numbers. These functions make it possible to construct more targeted searches without manually inspecting every packet.

For repeated investigations, display filters can be saved as bookmarks or converted into filter buttons. Bookmarks preserve useful 
expressions for later reuse, while filter buttons provide immediate access through a single click. Wireshark profiles extend this 
customization by storing configurations for different investigation requirements, including filtering buttons and coloring rules. 
Profiles can be created, modified, and selected through Edit → Configuration Profiles or the Profile section in the lower-right area 
of the status bar.

The official Wireshark references for further practice are the 
[Capture Filter Syntax documentation](https://wiki.wireshark.org/CaptureFilters?utm_source=chatgpt.com), the
[Display Filter Reference](https://www.wireshark.org/docs/dfref/?utm_source=chatgpt.com), and the 
[Display Filter Manual](https://www.wireshark.org/docs/man-pages/wireshark-filter?utm_source=chatgpt.com).&#x20;

[image](https://www.google.com/s2/favicons?domain=https://wiki.wireshark.org\&sz=32)

Wireshark Wiki+2

---

## Extracted Commands and Expressions

\| Description | Code/Command |\
\|---|---|\
\| Capture TCP traffic on port 80 | `tcp port 80` |\
\| Display TCP traffic on port 80 | `tcp.port == 80` |\
\| Equal | `==` |\
\| Not equal | `!=` |\
\| Greater than | `>` |\
\| Less than | `<` |\
\| Greater than or equal | `>=` |\
\| Less than or equal | `<=` |\
\| Equality example | `ip.src == <IP_ADDRESS>` |\
\| Inequality example | `ip.src != <IP_ADDRESS>` |\
\| Greater-than example | `ip.ttl > 250` |\
\| Less-than example | `ip.ttl < 10` |\
\| Greater-than-or-equal example | `ip.ttl >= 0xFA` |\
\| Less-than-or-equal example | `ip.ttl <= 0xA` |\
\| Logical AND | `(ip.src == <IP_ADDRESS>) && (ip.src == <IP_ADDRESS>)` |\
\| Logical OR | `(ip.src == <IP_ADDRESS>) || (ip.src == <IP_ADDRESS>)` |\
\| Logical NOT | `!(ip.src == <IP_ADDRESS>)` |\
\| All IP packets | `ip` |\
\| Packets involving an IP address | `ip.addr == <IP_ADDRESS>` |\
\| Packets involving a subnet | `ip.addr == <SUBNET>` |\
\| Source IP traffic | `ip.src == <IP_ADDRESS>` |\
\| Destination IP traffic | `ip.dst == <IP_ADDRESS>` |\
\| TCP port 80 | `tcp.port == 80` |\
\| UDP port 53 | `udp.port == 53` |\
\| TCP source port 1234 | `tcp.srcport == 1234` |\
\| UDP source port 1234 | `udp.srcport == 1234` |\
\| TCP destination port 80 | `tcp.dstport == 80` |\
\| UDP destination port 5353 | `udp.dstport == 5353` |\
\| All HTTP packets | `http` |\
\| All DNS packets | `dns` |\
\| HTTP response code 200 | `http.response.code == 200` |\
\| DNS requests | `dns.flags.response == 0` |\
\| HTTP GET requests | `http.request.method == "GET"` |\
\| DNS responses | `dns.flags.response == 1` |\
\| HTTP POST requests | `http.request.method == "POST"` |\
\| DNS A records | `dns.qry.type == 1` |\
\| Find Apache servers | `http.server contains "Apache"` |\
\| Find PHP and HTML pages | `http.host matches "\\.(php|html)"` |\
\| Find traffic using selected ports | `tcp.port in {80 443 8080}` |\
\| Convert server values to uppercase | `upper(http.server) contains "APACHE"` |\
\| Convert server values to lowercase | `lower(http.server) contains "apache"` |\
\| Find frames ending in odd numbers | `string(frame.number) matches "[13579]$"` |

---

## Extracted Tables

Comparison Operators

| English | C-like | Description           |
| ------- | ------ | --------------------- |
| eq      | `==`   | Equal                 |
| ne      | `!=`   | Not equal             |
| gt      | `>`    | Greater than          |
| lt      | `<`    | Less than             |
| ge      | `>=`   | Greater than or equal |
| le      | `<=`   | Less than or equal    |

Logical Operators

\| English | C-like | Description |\
\|---|---|---|\
\| and | `&&` | Logical AND |\
\| or | `||` | Logical OR |\
\| not | `!` | Logical NOT |

Filter Toolbar Indicators

| Colour | Meaning                                               |
| ------ | ----------------------------------------------------- |
| Green  | Valid filter                                          |
| Red    | Invalid filter                                        |
| Yellow | Warning filter; functional but potentially unreliable |

IP Filters

| Filter                    | Description                                                      |
| ------------------------- | ---------------------------------------------------------------- |
| `ip`                      | Display all IP packets                                           |
| `ip.addr == <IP_ADDRESS>` | Display packets containing the specified IP address              |
| `ip.addr == <SUBNET>`     | Display packets containing addresses within the specified subnet |
| `ip.src == <IP_ADDRESS>`  | Display packets originating from the specified address           |
| `ip.dst == <IP_ADDRESS>`  | Display packets sent to the specified address                    |

TCP and UDP Filters

| Filter                | Description                            |
| --------------------- | -------------------------------------- |
| `tcp.port == 80`      | TCP packets involving port 80          |
| `udp.port == 53`      | UDP packets involving port 53          |
| `tcp.srcport == 1234` | TCP packets originating from port 1234 |
| `udp.srcport == 1234` | UDP packets originating from port 1234 |
| `tcp.dstport == 80`   | TCP packets sent to port 80            |
| `udp.dstport == 5353` | UDP packets sent to port 5353          |

Application-Level Protocol Filters

| Filter                          | Description                         |
| ------------------------------- | ----------------------------------- |
| `http`                          | All HTTP packets                    |
| `dns`                           | All DNS packets                     |
| `http.response.code == 200`     | HTTP packets with response code 200 |
| `dns.flags.response == 0`       | DNS requests                        |
| `http.request.method == "GET"`  | HTTP GET requests                   |
| `dns.flags.response == 1`       | DNS responses                       |
| `http.request.method == "POST"` | HTTP POST requests                  |
| `dns.qry.type == 1`             | DNS A-record queries                |

Advanced Filtering Operators and Functions

| Filter     | Type                | Description                                               |
| ---------- | ------------------- | --------------------------------------------------------- |
| `contains` | Comparison operator | Searches for a case-sensitive value within a packet field |
| `matches`  | Comparison operator | Searches using a case-insensitive regular expression      |
| `in`       | Set membership      | Checks whether a value belongs to a specified set         |
| `upper`    | Function            | Converts a string to uppercase                            |
| `lower`    | Function            | Converts a string to lowercase                            |
| `string`   | Function            | Converts a non-string value into a string                 |

Capture Filter Syntax Components

| Component | Available values                                          |
| --------- | --------------------------------------------------------- |
| Scope     | `host`, `net`, `port`, `portrange`                        |
| Direction | `src`, `dst`, `src or dst`, `src and dst`                 |
| Protocol  | `ether`, `wlan`, `ip`, `ip6`, `arp`, `rarp`, `tcp`, `udp` |

---

## Key Takeaways

- Start the lab VM, open Wireshark, and load `Exercise.pcapng` from the desktop.
- Use the Statistics menu to establish the scope of the investigation.
- Examine Resolved Addresses, Protocol Hierarchy, Conversations, and Endpoints to identify relevant network activity.
- Enable IP and port name resolution when readable endpoint information is required.
- Configure MaxMind databases when IP geolocation information is needed.
- Review IPv4, IPv6, DNS, and HTTP statistics to narrow the investigation.
- Understand the distinction between capture filters and display filters. Configure capture filters before capturing; use display
  filters to investigate recorded or ongoing traffic.
- Learn capture filter components: scope, direction, and protocol.
- Apply comparison and logical operators to construct more precise display filters.
- Use the filter toolbar's green, red, and yellow indicators to evaluate filter validity.
- Apply IP, TCP, UDP, HTTP, and DNS filters according to the protocol information being investigated.
- Use Display Filter Expressions to identify available fields and acceptable values.
- Apply Coloring Rules to highlight relevant packets.
- Use `contains`, `matches`, and `in` for more advanced filtering.
- Use `upper`, `lower`, and `string` to transform field values before searching.
- Save frequently used expressions as bookmarks or filter buttons.
- Create separate Wireshark profiles for different investigation requirements, preserving their respective filtering and coloring
  configurations.

---



