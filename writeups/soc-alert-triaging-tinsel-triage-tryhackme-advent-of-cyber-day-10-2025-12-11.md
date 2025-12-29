# SOC Alert Triaging 

---
A sudden surge of security alerts, such as those indicating a potential compromise within an Azure tenant, necessitates a rapid and 
structured response centered on alert triaging. Jumping immediately into every incoming notification is inefficient, as a significant 
portion may represent noise or benign false positives. **Alert triaging** is the critical initial process designed to convert this 
operational chaos into clarity by prioritizing alerts that genuinely represent an active threat, thus ensuring the security operations 
center (SOC) team’s resources are directed toward the highest-risk incidents. 

### Effective triage relies on consistently assessing four fundamental dimensions to swiftly identify and evaluate alerts. 
- The **Severity Level** indicates the urgency and potential business impact, ranging from informational to critical. 

- **Timestamp and Frequency** analysis places the alert in a historical context, helping to determine if the activity is ongoing or 
part of a coordinated pattern. 

- The **Attack Stage** factor requires placing the alert within the broader cyber kill chain (e.g., reconnaissance, persistence, or data 
exfiltration), offering insight into the attacker’s current progress and objectives. 

- Finally, **Affected Asset** identification demands 
an assessment of the compromised system, user, or resource's operational importance, dictating the priority based on potential 
organizational impact. These four factors—Severity, Time, Context (Attack Stage), and Impact (Affected Asset)—provide the foundational 
framework for making informed, immediate decisions: escalation, deeper investigation, or closure.

Once high-priority alerts are identified, the deep investigation phase begins with detailed examination of the alert data, reviewing 
entities, event data, and the detection logic itself to confirm malicious behavior. This involves rigorously checking related log 
sources for corroborating patterns or unusual activity. **Correlation** is essential, linking multiple alerts that share entities 
(e.g., the same IP address or compromised user) to reconstruct a broader attack sequence and timeline. By combining timestamps and 
actions, a complete event timeline is built to determine the attack’s progression status. The resulting decision determines the next 
action: immediate escalation to incident response teams upon confirmed indicators of compromise, continued investigation for further 
evidence, or closure and rule suppression for confirmed false positives. Throughout this process, meticulous documentation of all 
findings, analysis, decisions, and remediation steps is mandatory for continuous SOC process improvement.

---
## Investigation Proper 

Security Operations Center (SOC) incident response begins with efficient alert triaging, a process essential for converting a high 
volume of threat notifications into actionable priorities. This process is executed within a Security Information and Event 
Management (SIEM) and Security Orchestration, Automation, and Response (SOAR) platform, such as **Microsoft Sentinel**. Sentinel, 
being a cloud-native solution, centralizes data collection from diverse Azure and external sources, enabling real-time detection, 
analysis, and threat response using AI and automation. Within Sentinel's **Incidents** management interface, alerts are automatically 
grouped into comprehensive incidents based on shared entities and timelines, facilitating a unified view of a potential attack. 

The triage process prioritizes incidents based on **Severity Level**, focusing immediately on alerts classified as High, which 
typically indicate critical compromise points or active privilege escalation attempts. By drilling into a high-severity incident, 
such as a **Linux PrivEsc—Kernel Module Insertion** alert, analysts can immediately infer key details:

* The number of underlying events
* The timestamp
* The involved entities (users, hosts, IP addresses)
* The corresponding MITRE ATT\&CK tactic (Privilege Escalation)

The **View full details** pane provides deeper context, including an incident timeline and links to similar occurrences, which 
are vital for contextualizing the attack.

A critical step in the investigation is **alert correlation** across multiple entities. When several distinct alerts point to the 
same asset (e.g., a specific Virtual Machine), it is highly probable that these alerts represent different stages of a single, 
coordinated intrusion rather than isolated, random events. The sequencing of events is crucial for tracing the attack path.

For example, if the same Linux host triggered the following alerts in succession, it reveals a comprehensive intrusion:

* **Root SSH Login from External IP:** Suggests Initial Access and system compromise.
* **SUID Discovery:** Indicates the attacker searched for ways to escalate privileges.
* **Kernel Module Insertion:** Represents the final stage where an attacker installed custom, privileged code
* (Persistence/Privilege Escalation) to achieve complete host control.

The **Kernel Module Insertion** specifically is a severe finding, suggesting an attacker has successfully loaded custom, 
privileged code directly into the kernel ring-0 space, demanding immediate and focused incident response. This structured 
triage allows the security team to move efficiently from initial notification to deep log analysis and containment.

---
## Diving Deeper Into Logs 

The initial triage of security alerts must transition immediately into a granular analysis of the raw log data within the Security 
Information and Event Management (SIEM) system, such as Microsoft Sentinel, to definitively validate the detections and reconstruct 
the attacker's actions. To achieve this, an analyst reviews the Events section within the full alert details view, which provides 
explicit evidence, such as the specific name of the installed kernel module and its installation timestamp on each compromised machine.

A more focused examination requires pivoting from the summarized alert data to raw events via custom queries. By switching the 
Sentinel view to KQL (Kusto Query Language) mode, the analyst can construct a precise query targeting all relevant events from a 
single host. Executing this custom query reveals a cluster of highly suspicious activities correlating with the kernel module 
insertion. The sequence of surrounding events, including actions that manipulate system files and privilege assignments, strongly 
confirms malicious intent and signifies a successful compromise.

The combined evidence—the insertion of a kernel module, coupled with the system-level changes—is indicative of post-exploitation 
activity focused on privilege escalation and persistence. Specifically, the execution of the `cp` command to back up the shadow file, 
the addition of a new user (`Alice`) to the `sudoers` group, the modification of an existing administrative user (`backupuser`) by the 
root account, and successful root SSH authentication collectively represent a severe deviation from normal operational baselines. This 
chain of events warrants immediate and intensive incident response efforts.

### Description,Code/Command
- KQL query for filtering logs by host,"`set query_now = datetime(2025-10-30T05:09:25.9886229Z); Syslog_CL | where host_s == 'app-02' | 
project _timestamp_t, host_s, Message`"
- Command used for shadow file backup,`cp` (copy)
- Name of the malicious kernel module,`malicious_mod.ko`

---
>[!Note]
>- Raw log analysis is essential for validating alerts and moving beyond high-level triage to uncover specific attacker actions.
>- Sentinel's KQL mode allows analysts to create custom queries for focused investigation into single entities (e.g., `app-02`).
>- Suspicious event sequences, particularly those manipulating user accounts and privilege groups, suggest privilege escalation 
and persistence.
>- Actions like creating shadow file backups, modifying administrative users, and inserting kernel modules are strong indicators 
of a successful, post-exploitation compromise.
>- Correlating events like successful root SSH authentication with privilege changes confirms highly unusual, likely malicious, 
system activity.

---
>[!Note]
>## You did it! Wareville is one step safer.
>The townsfolk are counting on you to keep Christmas secure.
>Head back to Wareville to continue your mission!






