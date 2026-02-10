# Introduction to EDR

---

<p align="center">
  <img src="images/edr_introduction-to-edr_tryhackme_2026-01-19.png" alt="EDR/Endpoint Detection And Response" 
  width="650"/>
</p>


Endpoint Detection and Response (EDR) monitors endpoints for threats, detecting and responding to advanced attacks. Widespread 
adoption stems from remote work exposing devices beyond network perimeters. Solutions like CrowdStrike Falcon, SentinelOne 
ActiveEDR, Microsoft Defender for Endpoint, OpenEDR, and Symantec EDR share similar architectures though features differ.

Core pillars include visibility through detailed telemetry—process modifications, registry changes, file and folder alterations, 
user actions—presented in structured formats like process trees with timelines and historical data for threat hunting. 
Detection exceeds signature matching by incorporating behavioral analysis, anomaly identification via machine learning, 
fileless malware spotting, custom indicators of compromise, and MITRE ATT&CK framework mapping. Response enables host isolation, 
process termination, file quarantine, remote shell access, and artefact extraction such as memory dumps, event logs, specific 
folder contents, or registry hives.

EDR outperforms traditional antivirus (AV) in depth; AV relies on signatures like passport checks at entry, while EDR conducts 
ongoing behavioral surveillance akin to internal security officers monitoring for deviations—roaming restricted areas, 
suspicious actions, unattended items. In phishing scenarios with malicious Visual Basic for Applications macros spawning 
obfuscated PowerShell to download payloads injected into svchost.exe for remote access, AV often misses steps lacking signatures, 
but EDR flags unusual parent-child relationships, script executions, process injections, and outbound connections, generating 
alerts with full chains.

Agents, or sensors, deploy on endpoints—Windows, Linux, Mac—to collect telemetry in real time, forwarding to a central console 
for correlation, machine learning analysis, and threat intelligence matching. The console dashboard displays detections by 
severity, time, file, hostname, username, and MITRE tactics/techniques, enabling triage. EDR integrates with security information
and event management (SIEM) alongside firewalls, data loss prevention (DLP), email gateways, and identity and access 
management (IAM) for ecosystem-wide visibility, though focused solely on hosts without network-level threat detection.

Telemetry encompasses process executions and terminations for suspicious relationships or payloads, network connections for 
command and control or exfiltration, command line activity in Command Prompt or PowerShell for malicious scripts, file 
modifications during staging or ransomware, and registry changes for Windows configurations. This data reconstructs timelines, 
identifies roots, and informs judgments.

Detection techniques feature behavioral monitoring for anomalies like Word spawning PowerShell, baseline deviation flagging 
such as auto-start registry edits, IOC matching for known hashes or executables, MITRE mapping to tactics like persistence 
via scheduled tasks, and machine learning for patterns in fileless or multi-stage attacks.

Response actions include network isolation to contain spread, process kills for targeted neutralization without full shutdown, 
quarantine relocating files to prevent execution, remote access for custom scripts or deeper inspection via Real Time Response 
in CrowdStrike Falcon, and artefact collection for forensics.

Simulation exercises triage by examining detections for false or true positives based on context, prioritizing higher 
severities.

I find EDR's behavioral focus particularly effective against evasion tactics, shifting from reactive signature hunts to 
proactive anomaly hunting.

---

| Attack Steps | AV's Response | EDR's Response |
|--------------|---------------|----------------|
| Step #1 | Does nothing if the downloaded file has no previous signature in the database | Logs the file download activity and monitors it |
| Step #2 | Does nothing upon the opening of the document since winword.exe is a legitimate utility | Records the execution of winword.exe and keeps monitoring |
| Step #3 | Does nothing if the executed macro has no previous signature | Detects and flags the macro execution due to the unusual parent-child relationship of winword.exe and PowerShell.exe processes |
| Step #4 | Typically, AVs will not detect obfuscated PowerShell scripts | Flags the obfuscated script execution |
| Step #5 | Will not flag malicious injection into svchost.exe since it does not monitor the memory injections | Detects Process Injection in svchost.exe |
| Step #6 | Lacks Network Level Visibility | Flags the unexpected behaviour of svchost.exe, making an outbound connection |
| Final Action | May be marked as clean | Generates an alert with the full attack chain and enables the analyst to take actions from within the EDR |

---

### Key Takeaways
- Deploy agents on endpoints to monitor activities and send telemetry to central console
- Correlate data with machine learning and threat intelligence for detections
- Acknowledge and prioritize alerts by severity in the console
- Triage to determine false or true positive based on detailed context
- Isolate hosts to contain malicious activity spread
- Terminate processes to neutralize threats without full isolation
- Quarantine files to prevent execution while allowing review
- Access shells remotely to execute custom actions or collect data
- Extract artefacts like memory dumps, event logs, folder contents, registry hives for forensics

---

### Gallery 

<p align="center">
  <table>
    <tr>
      <td><img src="images/edr_introduction-to-edr_tryhackme_2026-01-19.png" alt="EDR/Endpoint Detection And Response" 
  width="450"/>
      <td><img src="images/edr_visibility_introduction-to-edr_tryhackme_2026-01-19.png" alt="EDR Visibility" width="450"/></td>
    </tr>
    <tr>
      <td align="center"><strong>Figure 1a:</strong> EDR/Endpoint Detection And Response</td>
      <td align="center"><strong>Figure 1b:</strong> EDR Visibility</td>
    </tr>
    <tr>
      <td><img src="images/edr_detection_introduction-to-edr_tryhackme_2026-01-19.png" alt="EDR Detection" 
  width="450"/>
      <td><img src="images/edr_response_introduction-to-edr_tryhackme_2026-01-19.png" alt="EDR Response" width="450"/></td>
    </tr>
     <tr>
      <td align="center"><strong>Figure 2a:</strong> EDR Detection</td>
      <td align="center"><strong>Figure 2b:</strong> EDR Response</td>
    </tr>
  </table>
</p>


<p align="center">
  <table>
    <tr>
      <td><img src="images/edr_scenario-breakdown_introduction-to-edr_tryhackme_2026-01-19.png" alt="EDR Scenario Breakdown" 
  width="450"/>
      <td><img src="images/edr_scenario-breakdown-2_introduction-to-edr_tryhackme_2026-01-19.png" alt="EDR Scenario Breakdown 2" width="450"/></td>
    </tr>
    <tr>
      <td align="center"><strong>Figure 3a:</strong> EDR Scenario Breakdown</td>
      <td align="center"><strong>Figure 3b:</strong> EDR Scenario Breakdown 2</td>
    </tr>
    <tr>
      <td><img src="images/edr-agents-and-console_introduction-to-edr_tryhackme_2026-01-19.png" alt="EDR Agents And Console" 
  width="450"/>
      <td><img src="images/edr-console_introduction-to-edr_tryhackme_2026-01-19.png" alt="EDR Console" width="450"/></td>
    </tr>
     <tr>
      <td align="center"><strong>Figure 4a:</strong> EDR Agents And Console</td>
      <td align="center"><strong>Figure 4b:</strong> EDR Console</td>
    </tr>
  </table>
</p>



<p align="center">
  <table>
    <tr>
      <td><img src="images/edr-collected-telemetry_introduction-to-edr_tryhackme_2026-01-19.png" alt="EDR Collected Telemetry" 
  width="450"/>
      <td><img src="images/edr-dashboard-1_introduction-to-edr_tryhackme_2026-01-19.png" alt="EDR Dashboard 1" width="450"/></td>
    </tr>
    <tr>
      <td align="center"><strong>Figure 5a:</strong> EDR Collected Telemetry</td>
      <td align="center"><strong>Figure 5b:</strong> EDR Dashboard 1</td>
    </tr>
    <tr>
      <td><img src="images/edr-dashboard-2_introduction-to-edr_tryhackme_2026-01-19.png" alt="EDR Dashboard 2" 
  width="450"/>
      <td><img src="images/edr-dashboard-3_introduction-to-edr_tryhackme_2026-01-19.png" alt="EDR Dashboard 3" width="450"/></td>
    </tr>
     <tr>
      <td align="center"><strong>Figure 6a:</strong> EDR Dashboard 2</td>
      <td align="center"><strong>Figure 6b:</strong> EDR Dashboard 3</td>
    </tr>
  </table>
</p>



<p align="center">
  <table>
    <tr>
      <td><img src="images/endpoint-security_summary-1_introduction-to-edr_tryhackme_2026-01-19.png" alt="Endpoint Security Summary 1" 
  width="450"/>
      <td><img src="images/endpoint-security_actions-or-response_introduction-to-edr_tryhackme_2026-01-19.png" alt="Endpoint Security Actions Or Response" width="450"/></td>
    </tr>
    <tr>
      <td align="center"><strong>Figure 7a:</strong> Endpoint Security Summary 1</td>
      <td align="center"><strong>Figure 7b:</strong> Endpoint Security Actions Or Response</td>
    </tr>
    <tr>
      <td><img src="images/endpoint-security_summary-2_introduction-to-edr_tryhackme_2026-01-19.png" alt="Endpoint Security Summary 2" 
  width="450"/>
      <td><img src="images/endpoint-security_process-info_introduction-to-edr_tryhackme_2026-01-19.png" alt="Endpoint Security Process Info" width="450"/></td>
    </tr>
     <tr>
      <td align="center"><strong>Figure 8a:</strong> Endpoint Security Summary 2</td>
      <td align="center"><strong>Figure 8b:</strong> Endpoint Security Process Info</td>
    </tr>
  </table>
</p>


<p align="center">
  <table>
    <tr>
      <td><img src="images/endpoint-security_ioc-or-indicators_introduction-to-edr_tryhackme_2026-01-19.png" alt="Endpoint Security IOC Or Indicators" 
  width="450"/>
    </tr>
    <tr>
      <td align="center"><strong>Figure 9a:</strong> Endpoint Security IOC Or Indicators</td>
    </tr>
  </table>
</p>




---
