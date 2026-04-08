# Firewall Fundamentals

---

Firewalls serve as the digital security equivalent of guards positioned at building entrances to screen entrants and exits, preventing 
unpermitted access. They inspect all incoming and outgoing network traffic on devices or networks against configured rules to allow or 
block packets accordingly, with most current versions adding enhanced protective features beyond basic filtering.

Completion of the material delivered foundational knowledge on firewall types, rule composition and elements, hands-on use of the 
Windows built-in firewall, and Linux built-in firewall options. Familiarity with networking concepts was required beforehand.

Firewalls entered common network use once their effectiveness at filtering harmful traffic was established, resulting in multiple 
variants each aligned to particular roles and OSI model layers.

Stateless firewalls operate strictly at layers 3 and 4 by matching packets solely to preset rules without retaining any record of 
prior connections. Processing occurs rapidly since every packet is evaluated in isolation, yet this prevents application of policies 
that depend on connection context. Denial of initial packets from one source does not automatically extend to later ones, which are 
assessed anew.

Stateful firewalls extend beyond rule matching at the same layers by maintaining a state table of connections. Packets are judged 
against both rules and history, so an accepted connection from a source allows all subsequent packets without re-inspection while a 
denial blocks all future traffic from that source.

Proxy firewalls function at layer 7 as intermediaries between internal networks and the internet. They fully examine packet contents 
before forwarding approved requests under their own IP address to mask internal ones and enable content-based allow or deny policies.

Next-generation firewalls span layers 3 through 7 and incorporate deep packet inspection, real-time intrusion prevention to stop 
malicious activity immediately, heuristic analysis of attack patterns for preemptive blocking, and SSL/TLS decryption combined with 
threat intelligence feeds for precise decisions.

A table summarized distinguishing traits of each type to support selection for varied environments.

Rules grant precise oversight of traffic by supporting custom definitions, for instance restricting inbound access to approved IP 
addresses only while denying everything else.

Each rule incorporates the source IP address of the originating machine, destination IP address of the receiving machine, port number,
protocol used, action to apply, and traffic direction.

Actions specify the outcome once a packet matches: allow permits the traffic, deny rejects it to shrink exposure from malicious 
sources, and forward reroutes it to another segment when the firewall also performs routing. Directionality further categorizes rules
as inbound for incoming flows such as web server access on port 80, outbound for outgoing flows such as restricting port 25 except 
from mail servers, or forward for internal redirections such as sending port 80 traffic to an internal web server.

The Windows Defender Firewall, introduced by Microsoft in Windows systems, supplies core capabilities for managing program allowances
and custom traffic rules. The attached virtual machine was started in split view via the designated button or the show split view 
option if needed. Remote connection details were Administrator as username, ******** as password, and <MACHINE_IP> as address. 
Launching the interface required searching for Windows Defender Firewall in the start menu to reach the dashboard showing network 
profiles.

Two network profiles are available and selected automatically through network location awareness. Private networks apply to trusted 
home connections while guest or public networks cover untrusted environments such as public Wi-Fi, allowing configuration of stricter 
policies like blocking all inbound connections without impacting the private profile. Individual applications or features can be 
permitted or blocked per profile, the firewall itself toggled on or off, or all settings restored to defaults from the main view.

Custom rules are formed in advanced settings by opening the outbound rules section and launching the new rule wizard. For the 
demonstration, a rule was built to block outbound HTTP and HTTPS traffic by choosing the custom option, selecting all programs, 
specifying TCP protocol with remote ports 80 and 443 separated by commas, leaving IP scopes unchanged, enabling the block connection 
action, applying to all profiles, and naming the rule. The rule then listed under outbound rules, and subsequent attempts to reach 
http://<TEST_IP>/ failed as expected. An exercise required reviewing rules created on a critical Windows system to counter observed 
suspicious traffic.

On Linux the Netfilter framework delivers the underlying packet filtering, network address translation, and connection tracking that
powers multiple utilities. Common ones are iptables, the longstanding choice in most distributions, nftables as its enhanced successor,
firewalld with ready zone configurations, and ufw which simplifies rule creation as a front end to iptables.

Ufw commands streamline management: status checks, enabling for system startup, default outgoing policy settings to allow all unless 
restricted, targeted denials such as for port 22 TCP, numbered rule listings, and deletions by rule number. The appropriate utility 
depends on experience level and specific requirements, with direct rule testing advised to confirm behavior.

---

| Description | Code/Command |
|-------------|--------------|
| Check status of the ufw firewall | sudo ufw status |
| Enable the ufw firewall | sudo ufw enable |
| Set default policy to allow all outgoing traffic | sudo ufw default allow outgoing |
| Deny incoming SSH traffic on TCP port 22 | sudo ufw deny 22/tcp |
| List all active rules in numbered order | sudo ufw status numbered |
| Delete a specific rule by its number | sudo ufw delete 2 |

---

**Extracted Tables**

Firewall Characteristics

| Firewalls | Characteristics |
|-----------|-----------------|
| Stateless firewalls | - Basic filtering<br>- No track of previous connections<br>- Efficient for high-speed networks |
| Stateful firewalls | - Recognize traffic by patterns<br>- Complex rules can be applicable<br>- Monitor the network connections |
| firewalls | - Inspect the data inside the packets as well<br>- Provides content filtering options<br>- Provides application control<br>- Decrypts and inspects SSL/ data packets |
| Next-generation firewalls | - Provides advanced threat protection<br>- Comes with an intrusion prevention system<br>- Identify anomalies based on heuristic analysis<br>- Decrypts and inspects SSL/ data packets |

Allow Rule Example

| Action | Source | Destination | Protocol | Port | Direction |
|--------|--------|-------------|----------|------|-----------|
| Allow | <INTERNAL_SUBNET> | Any |  | 80 | Outbound |

Deny Rule Example

| Action | Source | Destination | Protocol | Port | Direction |
|--------|--------|-------------|----------|------|-----------|
| Deny | Any | <INTERNAL_SUBNET> |  | 22 | Inbound |

Forward Rule Example

| Action | Source | Destination | Protocol | Port | Direction |
|--------|--------|-------------|----------|------|-----------|
| Forward | Any | <WEB_SERVER_IP> |  | 80 | Inbound |

---

### Key Takeaways

- The types of firewalls
- The rules and its components
- Hands-on Windows built-in
- Hands-on built-in
- Private networks: configurations to apply when connected to home network
- Guest or public networks: configurations to apply when connected to untrusted network such as coffee shops, with options to block
  all incoming connections
- Source address: the machine’s IP address that would originate the traffic
- Destination address: the machine’s IP address that would receive the data
- Port: the port number for the traffic
- Protocol: the protocol that would be used during the communication
- Action: this defines the action that would be taken upon identifying any traffic of this particular nature
- Direction: this field defines the rule’s applicability to incoming or outgoing traffic
- Allow: the particular traffic defined inside the rule would be permitted
- Deny: the traffic defined inside the rule would be blocked and not permitted
- Forward: redirects traffic to a different network segment using the forwarding rules
- Inbound rules: meant to be applied to incoming traffic only
- Outbound rules: made for outgoing traffic only
- Forward rules: created to forward specific traffic inside the network
- Select the Custom option in the rule wizard and proceed to next
- Select All programs and proceed to next
- Set protocol type to TCP, keep local port as is, change remote port to Specific ports and enter 80,443 with no spaces between
  commas
- Keep local and remote IP addresses default and proceed
- Enable the Block the connection option and proceed
- Keep all network profiles selected and proceed
- Provide a name and optional description for the rule then finish
- To check ufw status run the status command
- To activate ufw use the enable command
- To set default outgoing policy use the default allow outgoing command
- To deny incoming SSH use the deny 22/tcp command
- To list active rules use the status numbered command
- To delete a rule use the delete command followed by the rule number

---






