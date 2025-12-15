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






