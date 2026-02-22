# Module 3: Introduction To Cybersecurity Tools

---

In my ongoing notes on foundational cybersecurity practices, I've been reflecting on the role of tools that help maintain organizational 
defenses. Security professionals rely on a range of applications to handle tasks like gathering data, spotting threats, and automating 
responses, all aimed at building a solid protective framework. Logs stand out as essential records capturing events across systems and 
networks, with types such as firewall entries tracking internet traffic, network logs noting device connections, and server logs 
documenting service activities. These provide the raw material for analysis. Security Information and Event Management (SIEM) systems 
pull together this log data for real-time oversight, enabling monitoring, detailed examination, and automatic notifications that cut 
down on manual work. Customizing these tools to fit an organization's unique requirements is key, as it allows for ongoing adjustments 
to counter emerging dangers.

Dashboards in SIEM setups transform complex data into visual formats like charts and graphs, making it straightforward to spot patterns.
When an alert triggers, these interfaces offer timelines, locations, and precise timestamps for suspicious events, supporting rapid 
evaluations. They can be tailored to highlight metrics relevant to different teams, such as overall network flow for routine operations.
Looking ahead, SIEM evolution ties into cloud environments, where vendors handle infrastructure for cloud-hosted versions, accessible 
online and suitable for groups avoiding heavy internal setups. Cloud-native variants, fully vendor-managed, exploit cloud strengths 
like reliability and expansion. With the rise of Internet of Things (IoT) devices expanding attack surfaces, and advancements in 
artificial intelligence (AI) and machine learning (ML) improving detection and storage, automation via Security Orchestration, 
Automation, and Response (SOAR) collections will streamline handling of routine incidents, leaving analysts to tackle unusual cases. 
Interconnected platforms are progressing, though not yet seamless.

Accessibility remains a core consideration in designing these tools, ensuring broad usability including for those with disabilities, 
which ultimately bolsters overall effectiveness. For instance, avoiding reliance on color alone for alerts prevents oversights, and 
incorporating diverse user feedback refines mitigations. Innovations often stem from addressing such specifics, much like how features 
for one group end up aiding many. Deployment options for SIEM include self-hosted setups where organizations manage everything 
internally for direct control over sensitive information, cloud-hosted ones maintained by providers for ease, hybrid models blending 
both for balanced benefits, and cloud-native designs optimized for cloud perks. Notable examples include Splunk Enterprise 
(https://www.splunk.com/en_us/products/splunk-enterprise.html), a self-hosted platform for real-time log retention, analysis, and 
searching; Splunk Cloud (https://www.splunk.com/en_us/products/splunk-cloud-platform.html), its cloud-based counterpart for similar 
functions in hybrid or full-cloud scenarios; and Google's Chronicle (now part of Google Security Operations, 
https://cloud.google.com/security/products/security-operations), a cloud-native tool focused on data retention, analysis, and log 
monitoring.

Open-source tools, often cost-free and collaborative, foster security through community contributions and allow extensive modifications. 
Proprietary alternatives, owned by entities and typically fee-based, limit changes to source code, requiring users to await vendor 
updates. A persistent myth suggests open-source is inferior or riskier, but broad access actually enables quick fixes by experts, 
making it resilient. Linux, an open-source operating system (core resources at https://www.kernel.org/), serves as an interface between
hardware and users, customizable via command-line for specific needs, with variants for targeted tasks. Suricata (https://suricata.io/),
developed by the Open Information Security Foundation (OISF, https://oisf.net/), functions as open-source software for network analysis
and threat detection, inspecting traffic for anomalies and generating logs, widely adopted in public and private sectors and integrable 
with SIEMs.

In practice, SIEM dashboards vary by tool. For Splunk, the security posture view covers 24-hour events and trends for real-time threat 
checks; the executive summary tracks long-term health for risk reduction; incident review flags high-risk patterns with timelines; and 
risk analysis spots behavioral shifts per object like users or IPs. Chronicle's enterprise insights highlight alerts with confidence 
scores and severity for IOCs; data ingestion monitors log processing success; IOC matches tracks top threats over time; the main 
dashboard summarizes ingestion, alerts, and events; rule detections stats high-occurrence incidents; and user sign-in overviews reveal 
access patterns for anomaly detection. These aid in prioritizing responses. Career paths in this field defy stereotypes—no need for 
elite coding or degrees, as defensive roles value research, adaptability, and relationships. Unique experiences often drive progress,
and seeking guidance while forging individual routes is vital.

---

### Key Takeaways
- Security tools address challenges like data collection, threat detection, and task automation for comprehensive protection.
- Logs track events; firewall logs record internet traffic, network logs device connections, and server logs service events.
- SIEM tools collect and analyze logs for real-time visibility, monitoring, analysis, and alerts, requiring customization for new
  threats.
- SIEM dashboards visualize data with charts, graphs, tables for pattern identification; used in incident response for timelines,
  locations, times.
- Dashboards customizable for metrics like network traffic volume.
- Current SIEM needs human analysis; future includes cloud-hosted (vendor-maintained, internet-accessed) and cloud-native
  (optimized for cloud scalability).
- Evolutions address IoT attack surfaces, AI/ML for better detection, visualization, storage; SOAR automates responses to common events.
- Accessibility ensures usability for diverse abilities, improving security effectiveness through inclusive design and user research.
- Self-hosted SIEM: organization installs, operates, maintains for data control.
- Cloud-hosted: provider-managed, internet-accessed, no internal infrastructure needed.
- Hybrid: combines self-hosted and cloud for balanced control and benefits.
- Cloud-native: vendor-managed, leverages cloud availability, flexibility, scalability.
- Splunk Enterprise: self-hosted for log retention, analysis, real-time searching.
- Splunk Cloud: cloud-hosted for log collection, searching, monitoring in hybrid/cloud environments.
- Google's Chronicle: cloud-native for data retention, analysis, searching, log monitoring.
- Open-source tools: free, collaborative, customizable, secure through community; source code and training available for modifications.
- Proprietary tools: owned, fee-based, limited modifications, vendor-controlled updates.
- Misconception: open-source less effective/safe, but exposure enables quick fixes.
- Linux: open-source OS, command-line customizable, manages hardware/software; multiple versions for tasks.
- Suricata: open-source for network analysis, threat detection; inspects traffic, generates logs; OISF-maintained, free, integrates
  with SIEMs.
- Splunk security posture dashboard: 24-hour events/trends for SOCs, real-time threat monitoring.
- Splunk executive summary: long-term health monitoring for risk reduction.
- Splunk incident review: identifies suspicious patterns, highlights high-risk items with timelines.
- Splunk risk analysis: identifies risks per object, behavioral changes.
- Chronicle enterprise insights: recent alerts, suspicious domains with confidence scores, severity.
- Chronicle data ingestion and health: event logs, sources, processing success rates.
- Chronicle IOC matches: top threats, trends over time for domains, IPs, devices.
- Chronicle main: high-level summary of ingestion, alerting, event activity timelines.
- Chronicle rule detections: stats on high-occurrence, severity, detections; lists alerts by rule.
- Chronicle user sign-in overview: user access behavior, sign-in events for anomalies.
- Chronicle analyzes logs by asset, domain, user, IP.
- Myths: no need for coding expertise, hacking, math prowess, degrees; blue team focuses on defense, diverse backgrounds valuable.
- Skills: relationship-building, quick learning, research, questioning, adaptability.
- Navigate career: seek support, embrace unique paths.
- Logs crucial for monitoring/auditing; types include firewall, network, server.
- SIEM dashboards for visual security posture insights.
- Common SIEM: Splunk, Chronicle.
- Chronicle: cloud-native tool to retain, analyze, search data.
- Incident response: quick identification, containment, correction of breaches.
- Log: record of system events.
- Metrics: attributes like response time, availability, failure rate for performance assessment.
- Operating system (OS): hardware-user interface.
- Playbook: manual for operational actions.
- Security information and event management (SIEM): application for monitoring critical activities.
- Security orchestration, automation, and response (SOAR): automated tools/workflows for event response.
- SIEM tools: platform collecting, analyzing, correlating IT data for real-time threat response, investigation, compliance.
- Splunk Cloud: cloud-hosted for log collection, search, monitoring.
- Splunk Enterprise: self-hosted for log retention, analysis, real-time security alerts.

---

### Gallery 

<p align="center">
  <table>
    <tr>
      <td><img src="../images/day-14-aoc-2025-defaced-website.png" alt="DoorDash website defaced with Hopperoo message after container escape" 
  width="450"/>
      <td><img src="../images/day-14-aoc-2025-restored-website.png" alt="Restored website" width="450"/></td>
    </tr>
    <tr>
      <td align="center"><strong>Figure 1a:</strong> Final defacement after container escape</td>
      <td align="center"><strong>Figure 1b:</strong> Restored website after running restoration script</td>
    </tr>
    <tr>
      <td><img src="../images/day-14-aoc-2025-deployer-bash-flag.png" alt="Using deployer bash to find the flag" 
  width="450"/>
     </tr>
     <tr>
      <td align="center"><strong>Figure 2a:</strong> Using deployer bash to find the flag</td>
    </tr>
  </table>
</p>

---






