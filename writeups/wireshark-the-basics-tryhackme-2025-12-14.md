# Wireshark: The Basics

---
## Wireshark: Core Functionality and Use Cases

Wireshark is an indispensable, open-source, and cross-platform network packet analyzer used extensively for sniffing live network 
traffic and conducting in-depth inspection of stored packet capture (PCAP) files. It serves as a fundamental tool for network 
engineers and cybersecurity analysts. While not an Intrusion Detection System (IDS), Wireshark allows analysts to discover, 
investigate, and decode raw packets, relying heavily on the user's protocol knowledge and investigative skill to identify network 
problems or security anomalies. 

### Common use cases for Wireshark include:

* **Network Troubleshooting:** Diagnosing issues like load failure points, congestion, and general connectivity problems.
* **Security Analysis:** Identifying anomalies such as rogue hosts, unusual port usage, and suspicious communication patterns.
* **Protocol Investigation:** Learning and validating protocol details, including response codes, packet structure, and payload data.

### Graphical User Interface (GUI) and Data Presentation

The Wireshark interface organizes analysis into several key sections:

* **Toolbar:** Contains menus and shortcuts for filtering, sorting, summarising, and handling packet captures.
* **Display Filter Bar:** The primary query section for filtering displayed packets.
* **Recent Files:** A list allowing quick recall of previously investigated captures.
* **Capture Filter and Interfaces:** Displays available network interfaces (e.g., `lo`, `eth0`) and allows configuration of
  capture filters.
* **Status Bar:** Provides tool status, active profile, and numerical packet statistics.

Once a PCAP file is loaded, Wireshark presents packet details across three primary panes, enabling multi-format analysis:

* **Packet List Pane:** Provides a high-level summary of each packet, including source/destination addresses, protocol, and basic
  information. Selecting a packet here populates the other two panes.
* **Packet Details Panel:** Displays a hierarchical, detailed breakdown of the selected packet's protocol layers.
* **Packet Bytes Pane:** Shows the hexadecimal and decoded ASCII representation of the selected packet, dynamically highlighting
  the specific bytes corresponding to fields selected in the Details Panel.

---
## Customization and Management Features

Wireshark uses **packet coloring** to help analysts quickly spot protocols and anomalies. Rules can be **permanent** (saved to a 
user profile) or **temporary** (available only for the current session) and are based on display filters.

Wireshark also offers traffic sniffing capabilities, controlled by dedicated toolbar buttons for starting (blue shark icon), 
stopping (red icon), and restarting (green icon) the capture process. 

For organizational purposes, analysts can **merge** two PCAP files into a single capture via the "File → Merge" menu path. 
Additionally, essential file metadata—including hash, capture time, comments, interface details, and statistics—can be recalled 
by navigating to "Statistics → Capture File Properties."

| Description | Interface Section |
| :--- | :--- |
| Primary query and filtering section | Display Filter Bar |
| Sniffing/processing menus and shortcuts | Toolbar |
| Available sniffing points (e.g., `eth0`) | Capture Filter and Interfaces |
| Summary of each packet (source, destination) | Packet List Pane |
| Detailed protocol breakdown | Packet Details Panel |

---

### Key Takeaways

* Wireshark is a powerful packet analyzer used for network troubleshooting and security anomaly detection, but it is not an IDS.
* The GUI is structured into five main components: Toolbar, Display Filter Bar, Recent Files, Capture Filter/Interfaces, and Status Bar.
* Packet data is presented in three panes: **Packet List**, **Packet Details**, and **Packet Bytes**, facilitating granular investigation.
* Packet coloring rules, both temporary and permanent, are used to quickly highlight protocols and events of interest.
* Wireshark supports traffic sniffing, merging of PCAP files, and detailed viewing of capture file properties.

---

>[!Tip]
>Knowing the file details is helpful. Especially when working with multiple pcap files, sometimes you will need to know and recall the file details (File hash, capture time, capture file comments, interface and statistics) to identify the file, classify and prioritise it. You can view the details by following "Statistics --> Capture File Properties" or by clicking the "pcap icon located on the left bottom" of the window.

Use the "Exercise.pcapng" file to answer the questions.
Read the "capture file comments". What is the flag?
- `TryHackMe_Wireshark_Demo`

What is the total number of packets?
- `58620`
  
What is the SHA256 hash value of the capture file?
- `446de335565fb0b0ee5e5a3266703c778b2f3dfad7efeaeccb2da5641a6d6eb`

---
## Packet Dissection and OSI Layer Analysis

Packet dissection, or protocol dissection, is the process by which a tool like Wireshark investigates the detailed contents of 
captured network traffic by decoding available protocols and their corresponding fields. Wireshark provides native support for an 
extensive list of protocols and allows for custom dissection scripts to be written for proprietary or unique protocols. 

The dissection process organizes the packet's information according to the **OSI model**, breaking down the data into hierarchical 
layers. When a packet is selected in the list pane, its details are presented, typically spanning five to seven logical sections 
that correspond to the OSI layers. Clicking on a specific detail within the protocol dissection pane automatically highlights the 
corresponding raw bytes in the packet bytes pane, demonstrating exactly where that data exists in the frame.

### A packet dissection typically includes the following layers:

* **The Frame (Layer 1 - Physical):** Contains information specific to the captured frame, representing details associated with
  the Physical layer of the OSI model.
* **Source [MAC] (Layer 2 - Data Link):** Displays the source and destination MAC addresses, information originating from the
  Data Link layer.
* **Source [IP] (Layer 3 - Network):** Provides the source and destination IPv4 addresses, corresponding to the Network layer.
* **Protocol (Layer 4 - Transport):** Details the protocol used (TCP or UDP) along with the source and destination port numbers,
  originating from the Transport layer.
* **Protocol Errors (Continuation of Layer 4):** This section specifically highlights segments related to TCP that required
  reassembly, indicating potential fragmentation or out-of-order delivery issues.
* **Application Protocol (Layer 5/7 - Application):** Shows details specific to the high-level protocol in use, such as HTTP,
  FTP, or SMB, which aligns with the Application layer of the OSI model.
* **Application Data (Extension of Layer 5/7):** An extension that reveals the application-specific data payload carried within
  the packet.

| Description | OSI Layer |
| :--- | :--- |
| The Frame/Packet details | Layer 1 (Physical) |
| Source and Destination MAC Addresses | Layer 2 (Data Link) |
| Source and Destination IP Addresses | Layer 3 (Network) |
| TCP/UDP, Source and Destination Ports | Layer 4 (Transport) |
| Protocol Errors (TCP reassembly) | Layer 4 (Transport) |
| High-level protocol (HTTP, FTP, SMB) | Layer 5/7 (Application) |
| Application-specific payload | Layer 5/7 (Application) |

---

### Key Takeaways

* Packet dissection decodes captured network traffic fields based on the structured **OSI model**.
* Wireshark visually links the hierarchical protocol details to the raw packet bytes.
* Packet dissection typically shows layers for **Frame** (L1), **MAC** (L2), **IP** (L3), **Protocol** (L4), and
  **Application** (L5/L7).
* The **Protocol Errors** section within Layer 4 highlights reassembled TCP segments, which is relevant for forensic analysis.

---
Use the "Exercise.pcapng" file to answer the questions. View packet number 38. Which markup language is used under the HTTP protocol?
- `eXtensible Markup Language`

What is the arrival date of the packet? (Answer format: Month/Day/Year)
- `05/13/2004`

What is the TTL value?
- `47`

What is the TCP payload size?
- `424`

What is the e-tag value?
(For example: 82ecb-6321-9e904585)
- `9a01a-4696-7e354b00`

---
## Packet Navigation

### Advanced Packet Analysis and Workflow Features

Wireshark assigns a unique sequential number to every packet within a capture, simplifying the tracking of large datasets and 
enabling quick navigation to specific events. The **Go to Packet** function, accessible via the toolbar or the **Go** menu, 
allows for precise navigation, not just by packet number, but also by tracking conversational context, moving between related 
frames within a session. 

Beyond simple numerical access, the **Find Packet** feature permits content-based searching across the capture. This functionality 
is crucial for identifying specific intrusion patterns or network failure traces. The search accepts four input types: Display 
Filter, Hex, String, and Regular Expression (Regex), with String and Regex being the most common. Searches are case-insensitive 
by default but can be made case-sensitive. Critically, the user must specify the correct search field, choosing between the 
Packet List, Packet Details, or Packet Bytes panes, as searching in the wrong pane will prevent a successful match even if the 
content exists.

Workflow efficiency is enhanced by **Marking Packets** and **Packet Comments**. Marking packets flags specific frames in black 
for immediate visual reference, facilitating later investigation or scoped exportation. However, marked packet information is 
ephemeral and is reset upon closing the capture file. In contrast, **Packet Comments** allow analysts to add persistent notes 
to individual packets, which are saved within the capture file and are visible to subsequent investigators.

For data management, Wireshark supports the **Export Packets** function, necessary when isolating a limited scope of suspicious 
traffic from a large file for deeper incident analysis. This ensures only relevant data is shared, eliminating extraneous 
information. Additionally, the **Export Objects (Files)** feature allows analysts to extract files transferred over specific 
application protocol streams—such as DICOM, HTTP, IMF, SMB, and TFTP—for malware analysis or digital forensics.

The **Time Display Format** is a critical setting, as Wireshark defaults to displaying time as "Seconds Since Beginning of 
Capture." For proper forensic correlation and a more conventional view, changing this to **UTC Time Display Format** is 
commonly recommended via the **View** menu.

Finally, Wireshark provides **Expert Info**, a built-in feature that actively detects protocol states and suggests possible 
anomalies or problems to the analyst.  While these are only suggestions and require manual validation to mitigate false 
positives/negatives, they are categorized by severity and function: **Chat** (Blue - informational workflow), **Note** 
(Cyan - notable events like application errors), **Warn** (Yellow - unusual errors or problem statements), and **Error** 
(Red - definitive problems like malformed packets). Common groups include `Checksum` errors, `Deprecated` protocol usage, 
`Comment` detection, and `Malformed` packet detection. The Expert Information dialog box, accessible via the Status Bar or 
the **Analyze** menu, provides a consolidated view of all detected entries by packet number, summary, and protocol group.

| Description | Input Types for Find Packet |
| :--- | :--- |
| Search by content inside packets | Display filter, Hex, String, Regex |
| Command to access Packet Find menu | `Edit --> Find Packet` |

---

### Key Takeaways

* **Packet Numbers** facilitate tracking, while the **Go to Packet** function enables conversation-aware navigation.
* The **Find Packet** feature supports four input types (Filter, Hex, String, Regex) but requires selection of the correct search
  pane (List, Details, Bytes).
* **Marked Packets** (ephemeral) visually flag events, while **Packet Comments** (persistent) save notes within the capture file.
* **Export Packets** and **Export Objects** are used to scope data for further analysis, isolating suspicious traffic or
  extracted files from supported protocols (DICOM, HTTP, IMF, SMB, TFTP).
* Switching the **Time Display Format** to UTC is a best practice for forensic correlation.
* **Expert Info** (categorized as Chat, Note, Warn, Error) provides automated protocol state detection and suggests potential
  anomalies.

---
Use the "Exercise.pcapng" file to answer the questions. Search the "r4w" string in packet details. What is the name of artist 1?
- `r4w8173`

Go to packet 12 and read the packet comments. What is the answer?
Note: use md5sum <filename> terminal command to get MD5 hash
- `911cd574a42865a956ccde2d04495ebf`

There is a ".txt" file inside the capture file. Find the file and read it; what is the alien's name?
- `PACKETMASTER`

Look at the expert info section. What is the number of warnings?
- `1636`

---
## Wireshark Filtering and Advanced Analysis

Wireshark employs a powerful filter engine categorized into two approaches: **Capture Filters**, which constrain the packets 
recorded during the sniffing process, and **Display Filters**, which selectively show already captured packets. While specific 
queries using Wireshark’s protocol reference are the primary method, the graphical interface provides convenient, query-free 
filtering methods. A key principle is that if a field can be selected in the details pane, it can be filtered or copied.

The most basic method is **Apply as Filter**, accessible via the right-click menu or the `Analyse` menu. This instantly generates 
and applies a display filter for the single, specific value selected, hiding all other packets and updating the packet count in 
the status bar. When an analyst needs to focus on a complete communication session, the **Conversation Filter** is utilized. 
This option filters all related packets based on source and destination addresses (IP and port numbers) linked to the selected 
packet, easily isolating a specific conversation stream. 

An alternative to filtering is **Colourise Conversation**. This highlights all linked packets in a specific color—overriding 
default coloring rules—without applying a display filter or reducing the number of visible packets. This provides a quick visual 
reference that can be reset via the `View` menu. The **Prepare as Filter** option differs from **Apply as Filter** in that it 
populates the display filter bar with the generated query but does not execute it immediately, allowing the analyst to combine 
it with other logic (using the `.. and/or ..` context menu options) before application.

Beyond filtering, Wireshark allows customization of the visual output. **Apply as Column** lets the analyst add any selected 
field from the packet details pane as a new column in the packet list pane. This is useful for horizontally tracking the 
appearance and value of a specific field across multiple packets.

For high-level application data reconstruction, **Follow Stream** is essential. Wireshark, by default, displays traffic in 
packet segments, but the Follow Stream function reconstructs the application-layer payload, presenting the data as it was 
sent. This is vital for viewing unencrypted data, such as credentials, and understanding the complete application flow. The 
server-originated data is highlighted in blue, and client-originated data is in red. Executing a Follow Stream command 
automatically generates and applies a corresponding display filter to isolate that stream; this filter must be manually cleared 
using the 'X' button in the display filter bar to return to the full packet list.

---

### Key Takeaways

* **Capture Filters** restrict traffic recorded; **Display Filters** restrict traffic viewed.
* **Apply as Filter** immediately filters for a selected field value.
* **Conversation Filter** isolates all packets belonging to a specific conversation (IP and port pairs).
* **Colourise Conversation** visually highlights a conversation without applying a display filter.
* **Prepare as Filter** stages a query in the filter bar for manual combination with other logic before execution.
* **Apply as Column** adds a selected field's value to the packet list pane for cross-packet tracking.
* **Follow Stream** reconstructs segmented protocol data (TCP/UDP/HTTP) into an application-level stream, crucial for viewing
  unencrypted payloads.
* Stream reconstruction results are color-coded (blue for server, red for client) and automatically apply a temporary display filter.

---
Use the "Exercise.pcapng" file to answer the questions.
Go to packet number 4. Right-click on the "Hypertext Transfer Protocol" and apply it as a filter.
Now, look at the filter pane. What is the filter query?
- `http`

What is the number of displayed packets?
- `1089`

Go to packet number 33790, follow the HTTP stream, and look carefully at the responses.
Looking at the web server's response, what is the total number of artists?
- `3`

What is the name of the second artist?
- `Blad3`

---

>[!Note]
>## You did it! 🎉 Wireshark: The Basics complete!








