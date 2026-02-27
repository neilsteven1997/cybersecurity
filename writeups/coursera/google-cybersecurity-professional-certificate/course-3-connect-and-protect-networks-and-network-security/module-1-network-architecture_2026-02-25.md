# Module 1: Network Architecture 

---

In reviewing the foundational aspects of network security as part of my ongoing study in the Google Cybersecurity Certificate, I noted 
how the course emphasizes the rising threats from network-based attacks, prompting a deep dive into network architecture and tools. The 
material starts with an introduction to network basics, where a network emerges as a group of interconnected devices exchanging data, 
whether through wired or wireless means. Devices rely on unique identifiers like IP addresses for routing across broader expanses and 
MAC addresses for local interactions, which struck me as a elegant way to ensure precise communication without overlap.

The course outlines its structure across four modules, beginning with network architecture, where I explored the shift toward cloud 
networks as a cost-effective alternative to traditional on-premise setups. Cloud computing stands out here, involving remote servers 
and services accessed via the internet, often through providers offering scalability and reduced maintenance burdens. This ties into 
hybrid environments that blend local and cloud resources, and software-defined networks that virtualize components like switches and 
routers for greater flexibility. I found the reliability aspect particularly resonant, as it minimizes downtime through distributed data
centers, though it demands vigilant security to verify traffic and user authenticity.

Delving into device roles, hubs broadcast data indiscriminately, making them less secure and largely obsolete in favor of switches, 
which direct packets intelligently using MAC address tables. Routers connect disparate networks, forwarding based on IP details, while 
modems bridge to internet service providers, and firewalls act as initial barriers by filtering traffic per configured rules. Servers 
follow a client-server model, responding to requests from devices like laptops or phones, and wireless access points enable Wi-Fi 
connections using radio waves. Virtualization extends this, allowing software to mimic hardware for efficiency, a concept I've seen 
pay off in resource allocation during peak loads.

Communication fundamentals revolve around data packets, the core units carrying headers with source and destination info, protocol 
indicators, and payloads, akin to enveloped messages. Performance hinges on bandwidth for data volume and speed for transfer rate, 
with anomalies often signaling intrusions. The TCP/IP model provides the framework for this, with its four layers: network access 
handling physical transmission, internet managing IP routing, transport overseeing flow via TCP for reliable connections or UDP for 
faster, less tracked ones, and application defining service interactions like HTTP for web or SMTP for email.

For deeper granularity, the OSI model expands to seven layers, starting from application where user-facing protocols operate, through 
presentation for data formatting and encryption like SSL, session for connection management, transport for segmentation and flow 
control, network for inter-network routing, data link for local framing via switches, and physical for hardware like cables. This 
model's detail aids in pinpointing issues, contrasting with TCP/IP's streamlined approach, though both conceptualize data flow 
effectively. IP packets themselves include headers with fields like version, length, TTL to prevent endless loops, and checksums for 
integrity, differing between IPv4's numeric format supporting billions of addresses and IPv6's hexadecimal expansion for vastly more.

Professionals shared insights that reinforced the human element: one CISO highlighted transitioning from development to defense after 
facing attacks, stressing curiosity and networking; a software engineer focused on collaborative coding for secure tools; an offensive 
engineer emphasized CLI proficiency, log parsing, and traffic analysis for vulnerability hunting, all underscoring communication to 
bridge technical and business needs. As I reflect, this groundwork equips me to anticipate exploits in data transit, where vulnerable
points like unpatched devices invite risks, and monitoring metrics becomes key to resilience.

The glossary consolidates terms like bandwidth as max data capacity, cloud network as remote resource storage, and packet sniffing as 
traffic inspection, which I'll reference often. For community engagement, the Coursera private Cybersecurity Community at 
https://www.coursera.org/learn/connect-and-protect-networks-and-network-security/discussions proves useful, alongside the quick start
guide there. Adhering to the Coursera Code of Conduct at https://www.coursera.org/about/terms/code-of-conduct ensures productive 
interactions. Resources like the certificate glossary template via Google Docs or direct download support retention, and feedback 
mechanisms via thumbs icons help refine content.

---

### Key Takeaways
- Course covers network architecture, operations, intrusion tactics, vulnerabilities, attacks, and securing networks through protocols,
   firewalls, VPNs, and hardening practices.
- Program includes nine courses: Foundations of Cybersecurity on profession and roles; Play It Safe on frameworks and tools; this
  Networks course on vulnerabilities and security; Tools of the Trade on Linux and SQL; Assets, Threats, and Vulnerabilities on controls
   and threats; Sound the Alarm on incident response; Automate with Python on coding tasks; Put It to Work on jobs and AI; Accelerate
  Job Search with AI on strategies and tools like Gemini.
- Module 1 focuses on network security threats, architecture, and securing mechanisms.
- Module 2 explores protocols, vulnerabilities in communication, and measures like firewalls for safe operations.
- Module 3 addresses attack types, securing compromised systems, and closing infrastructure loopholes.
- Module 4 covers hardening practices against intrusions, including cloud challenges.
- Learning opportunities: videos on concepts and tools; readings on topics and resources; community at Coursera for discussions;
  self-review labs for practice; quizzes for comprehension and grading (80% pass required).
- Tips for success: proceed in order; participate fully; replay confusing parts; bookmark links; follow Code of Conduct.
- To obtain certificate: pass all assignments at 80%; pay fee, get financial aid, or via sponsor.
- Healthy habits: plan study time; work at own pace; be curious with Gemini; take notes; review exemplars; build career identity;
  connect in community; update profile.
- Resources: Microsoft Word help at https://support.microsoft.com/en-us/word; Google Docs at https://support.google.com/docs; similar
  for Excel, Sheets, PowerPoint, Slides; Qwiklabs troubleshooting; glossaries in modules, courses, and certificate (downloadable).
- Network types: LAN for small areas; WAN for large, like the internet.
- Devices: hubs broadcast to all; switches direct via MAC; routers connect networks via IP; modems to ISP; firewalls monitor traffic;
  servers provide services in client-server model; wireless access points for Wi-Fi.
- Cloud services: SaaS for remote software; IaaS for virtual components; PaaS for custom apps.
- Benefits: reliability via availability; cost savings over on-premise; scalability on demand.
- TCP/IP layers: network access for packets and hardware; internet for IP addressing; transport for TCP/UDP flow; application for
  services like HTTP, SMTP, SSH, FTP, DNS.
- OSI layers: application for user protocols; presentation for translation/encryption; session for connections; transport for
  delivery/segmentation; network for routing; data link for local framing; physical for hardware.
- Protocols: ARP at network access; IP, ICMP at internet; TCP, UDP at transport.
- IPv4 packet fields: version, header length, ToS, total length, identification, flags, offset, TTL, protocol, checksum, source/dest IP,
   options.
- Differences: IPv4 32-bit numeric; IPv6 128-bit hex with flow label, more efficient, secure routing.
- Glossary terms: bandwidth (bits/sec max); cloud computing (remote services); data packet (unit of info); hub (broadcasts); IP
  (routing standards); IP address (device locator); LAN (small area); MAC address (device ID); modem (internet connector); OSI model
   (7 layers); packet sniffing (inspection); port (data organizer); router (connects networks); speed (transfer rate); switch (directs
  traffic); TCP/IP model (4 layers); TCP (connected streaming); UDP (connectionless); WAN (large area).

---

### Gallery 

<p align="center">
  <table>
    <tr>
      <td align="center"><img src="images/day-14-aoc-2025-defaced-website.png" alt="DoorDash website defaced with Hopperoo message after container escape" 
  width="450"/>
      <td align="center"><img src="images/day-14-aoc-2025-restored-website.png" alt="Restored website" width="450"/></td>
    </tr>
    <tr>
      <td align="center"><strong>Figure 1a:</strong> Final defacement after container escape</td>
      <td align="center"><strong>Figure 1b:</strong> Restored website after running restoration script</td>
    </tr>
    <tr>
      <td align="center"><img src="images/day-14-aoc-2025-deployer-bash-flag.png" alt="Using deployer bash to find the flag" 
  width="450"/>
      <td align="center"><img src="images/day-14-aoc-2025-secret-code.png" alt="Finding secret code by incrementing the number on website link" width="450"/></td>
    </tr>
     <tr>
      <td align="center"><strong>Figure 2a:</strong> Using deployer bash to find the flag</td>
      <td align="center"><strong>Figure 2b:</strong> Incrementing the number on link to find secret code</td>
    </tr>
  </table>
</p>



---






























































































