# Module 3: Secure Against Network Intrusions

---

Module 3 examined strategies for defending networks against intrusions, drawing directly from foundational network concepts to 
address how attackers target stored information through unauthorized access methods. Security analysts implement layered 
protections to shield sensitive data, recognizing that networks carry constant risks from malware deployment, spoofing 
techniques, and packet sniffing that enable traffic interception or disruption via flooding. The 2014 Home Depot breach 
illustrated the scale of such damage when malware compromised servers and exposed credit and debit card details belonging to 
over 56 million customers, resulting in operational interruptions, reputational harm, and lost customer trust.

Every network contains exploitable weaknesses that malicious actors pursue for financial, personal, political, or ideological 
reasons, including disgruntled insiders seeking to undermine operations. Network interception attacks capture data in transit 
through packet sniffing tools that reveal unauthorized information or allow modifications, such as redirecting funds in a bank 
transfer, while also permitting insertion of malicious code that halts normal functions. Backdoor attacks exploit deliberately 
created bypasses in access controls, originally meant for legitimate troubleshooting by programmers or administrators, but 
repurposed post-compromise to maintain persistence and enable actions like malware installation, denial of service floods, 
private data exfiltration, or reconfiguration that exposes systems further.

Consequences for organizations span financial losses from revenue-stopping downtime, infrastructure rebuilding, ransomware 
payments, and client litigation over breached personal data. Reputation suffers when incidents become public, eroding confidence
and driving customers to competitors. In government contexts, intrusions into power grids, water systems, or military 
communications can endanger public welfare, prompting global defense investments against cyber tactics.

A video segment with Matt, Google's chaos specialist, traced his path from lifeguarding, EMT work, and firefighting to 
cybersecurity, where he applies crisis-response instincts to threats ranging from ransomware and industrial espionage to 
state-sponsored intelligence operations, including adversaries using fabricated researcher personas on social media to deliver 
malware. Effective incident handling counters initial panic through the 3 Cs: establishing command for leadership, control for 
coordination, and communications for transparent information flow.

Denial of service attacks overwhelm networks or servers with excessive traffic to render them unresponsive to legitimate users,
amplifying financial exposure and opening doors to secondary exploits, while distributed variants leverage multiple remote 
devices for heightened impact. Network-level examples include SYN flood attacks that exploit the TCP handshake by bombarding 
servers with SYN packets until available ports are exhausted, ICMP flood attacks that exhaust bandwidth through repeated 
request-response cycles, and the ping of death that transmits ICMP packets exceeding 64 kilobytes to crash vulnerable systems.

tcpdump functions as a lightweight, command-line network protocol analyzer built on the open-source libpcap library, compatible
with Linux, Unix-based systems, and macOS, delivering human-readable packet details without heavy resource demands. Output from 
captures begins with timestamps in hours, minutes, seconds, and fractional seconds, followed by source and destination IP 
addresses, originating ports, and target ports, though default behavior resolves hosts to names and ports to service labels. 
Analysts rely on it to baseline traffic patterns, monitor utilization, detect malicious flows, generate alerts for anomalies, 
and pinpoint unauthorized messaging or wireless access, while remaining aware that adversaries can repurpose the same tools to 
harvest credentials and sensitive payloads.

The October 21, 2016 DDoS assault on a major DNS service provider, which hosted domain resolution for numerous high-traffic 
organizations, demonstrated volumetric impact when a botnet—originally assembled by university students for gaming targets and 
later released publicly with remote-control code—unleashed tens of millions of DNS requests at 7:00 a.m., collapsing resolution
services across North America and Europe. Affected sites became unreachable as user devices failed to map URLs to IP addresses,
though the provider restored operations within two hours and deflected subsequent waves through prepared mitigations.

Packet sniffing captures and inspects data packets containing messages, financial records, or payment details, allowing passive
observation or active manipulation in transit akin to tampering with private correspondence. IP spoofing disguises an attacker's
source address to impersonate trusted systems, evading firewall restrictions and facilitating unauthorized entry. Associated 
variants encompass on-path attacks that position the adversary between trusted endpoints to intercept or modify exchanges, 
including DNS lookups that redirect users to malicious destinations; replay attacks that delay or retransmit captured packets 
to mimic authorized sessions; and smurf attacks that spoof IPs before flooding broadcast addresses with ICMP echoes to amplify
denial of service across entire networks.

Network interface cards normally accept only addressed packets based on MAC matching and protocol processing, but when switched
to promiscuous mode they ingest all traffic for capture and storage, often via tools such as Wireshark, granting attackers IP 
and MAC details for spoofing follow-ons. Firewalls counter spoofing by dropping external packets claiming internal addresses or
suspicious patterns, while next-generation models monitor for broadcast anomalies. Encryption in transit, such as through TLS or
VPN tunnels, renders intercepted data unreadable, and defense-in-depth layers multiple controls since no single measure suffices
against every vector. DoS executions flood targets with seemingly authorized packets until servers cease legitimate responses, 
reinforcing the value of traffic analysis for rapid triage.

The module synthesized these elements, underscoring that packet sniffing and IP spoofing form core interception tactics, 
alongside DoS and DDoS methods like ICMP flooding, SYN attacks, and ping of death that saturate resources with unwanted data.

---

### Key Takeaways
- Malicious actors continually scan for emerging vulnerabilities and deploy backdoor methods alongside network interception
  attacks to extract sensitive data or inflict operational harm.
- Network attacks produce financial strain through downtime and recovery expenses, reputational erosion from lost public trust,
  and potential public safety threats when critical infrastructure such as power or defense systems is compromised.
- Security analysts must maintain ongoing education to identify patterns in traffic logs and apply timely mitigations that
  reduce both the probability and severity of intrusions.
- tcpdump and similar protocol analyzers output essential packet metadata including timestamps, source and destination IP
  addresses, and port numbers to support baseline establishment, malicious traffic detection, and alert creation, though the
  same capabilities enable credential harvesting by adversaries.
- Effective DDoS defenses incorporate dynamically scalable infrastructure across multiple hosts so core operations persist even
  if primary systems go offline.
- Analyzing IP packets and network logs provides the foundation for hypothesizing attack origins, guiding deeper investigation,
  and fortifying overall network posture.
- Prevention of packet sniffing relies on VPN encryption, HTTPS enforcement, and avoidance of unsecured public Wi-Fi to protect
  data in transit.
- IP spoofing countermeasures include TLS encryption, firewall rules that reject internet-sourced packets matching internal
  addresses, and next-generation firewalls capable of anomaly detection for oversized broadcasts.
- Defense-in-depth combines encryption, configuration hardening, and traffic monitoring rather than depending on any isolated
  control.
- Common network protocol analyzers encompass SolarWinds NetFlow Traffic Analyzer, ManageEngine OpManager, Azure Network Watcher,
  Wireshark, and tcpdump, with the latter favored for its terminal-based efficiency on Unix-like environments.

---

### Gallery 

<p align="center">
  <table>
    <tr>
      <td align="center"><img src="../images/activity-analyze-network-layer-communication_module-3-secure-against-network-intrusions_2026-03-06.png" alt="Activity: Analyze Network Layer Communication" 
  width="450"/>
      <td align="center"><img src="../images/test-your-knowledge-secure-networks-against-denial-of-service-dos-attacks_module-3-secure-against-network-intrusions_2026-03-06.png" alt="Test Your Knowledge: Secure Networks Against Denial Of Service (DOS) Attacks" width="450"/></td>
    </tr>
    <tr>
      <td align="center"><strong>Figure 1a:</strong> Activity: Analyze Network Layer Communication</td>
      <td align="center"><strong>Figure 1b:</strong> Test Your Knowledge: Secure Networks Against Denial Of Service (DOS) Attacks</td>
    </tr>
    <tr>
      <td align="center"><img src="../images/activity-analyze-network-attacks_module-3-secure-against-network-intrusions_2026-03-06.png" alt="Activity: Analyze Network Attacks" 
  width="450"/>
      <td align="center"><img src="../images/test-your-knowledge-network-interception-attack-tactics_module-3-secure-against-network-intrusions_2026-03-06.png" alt="Test Your Knowledge: Network Interception Attack Tactics" width="450"/></td>
    </tr>
     <tr>
      <td align="center"><strong>Figure 2a:</strong> Activity: Analyze Network Attacks</td>
      <td align="center"><strong>Figure 2b:</strong> Test Your Knowledge: Network Interception Attack Tactics</td>
    </tr>
  </table>
</p>



<p align="center">
  <table>
    <tr>
      <td align="center"><img src="../images/module-3-challenge-graded-assignment_module-3-secure-against-network-intrusions_2026-03-06.png" alt="Module 3 Challenge : Graded Assignment" 
  width="450"/>
    </tr>
    <tr>
      <td align="center"><strong>Figure 3a:</strong> Module 3 Challenge : Graded Assignment</td>
    </tr>
  </table>
</p>



---


