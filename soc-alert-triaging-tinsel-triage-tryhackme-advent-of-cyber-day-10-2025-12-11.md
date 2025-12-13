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





