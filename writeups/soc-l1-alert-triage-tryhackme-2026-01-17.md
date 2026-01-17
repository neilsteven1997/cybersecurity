# SOC L1 Alert Triage

---
Alert triage stands as the core workflow for SOC Level 1 analysts, transforming raw event data into actionable insights that 
distinguish benign noise from active breaches. Events—user authentications, process creations, or network transfers—generate 
logs across operating systems, firewalls, and cloud environments, forwarded to centralized platforms for analysis. Security 
solutions like SIEM or EDR ingest these logs, applying detection rules to produce alerts only for anomalous sequences, sparing 
teams from sifting through millions of routine entries daily.

Platforms for managing alerts vary by scale. SIEM systems such as Splunk Enterprise Security or Elastic handle core aggregation 
effectively. EDR and NDR tools like Microsoft Defender or CrowdStrike provide native dashboards, though integration into SIEM 
or SOAR is preferred for unified views. Larger teams leverage SOAR like Splunk SOAR or Cortex XSOAR for cross-tool orchestration, 
while custom ITSM setups using Jira or TheHive adapt for ticketing needs—I've used Trello in small ops for similar lightweight 
tracking.

Level 1 analysts lead triage, reviewing alerts to classify and escalate threats, while Level 2 conducts deeper remediation. 
Engineers tune rules to ensure alerts carry sufficient context, and managers monitor throughput to avoid misses. Essential 
alert attributes include creation time (lagging slightly behind the event), descriptive name, severity rating (low to critical), 
status (new, in progress, closed), verdict (true or false positive), assignee for accountability, rule-based description 
explaining trigger logic and triage notes, and fields capturing indicators like hostname or command line.

Prioritization ensures timely handling amid queue buildup. Filter to new, unassigned alerts only. Order by severity 
descending—critical alerts flag higher-impact risks per engineering design—then by age ascending, addressing older incidents 
first where adversaries may already dwell.

Triage unfolds in phases. Claim ownership by assigning to self and marking in progress, then review name, description, and 
indicators for orientation. Investigation demands technical scrutiny: identify affected assets, dissect the flagged action, 
examine proximal events for chains, and validate with threat intelligence. Without formal playbooks, these steps guide 
consistency. Conclude with verdict assignment—true positive for malicious, false for benign—detailed commentary on methodology 
and rationale, then closure.

The simulation in TryHackMe's SIEM dashboard reinforces these mechanics; mismatched verdicts or statuses block progress, 
underscoring precision. Mastery here builds intuition for escalation and reporting ahead.

---

Key Takeaways
- Generate alerts from logged events via SIEM, EDR, NDR, SOAR, or ITSM platforms for focused analysis
- Involve L1 in initial review and escalation, L2 in advanced investigation, engineers in rule tuning, managers in quality
  oversight
- Track alert time, name, severity, status, verdict, assignee, description, and fields for comprehensive context
- Prioritize by filtering new alerts, sorting severity descending, then time ascending to address risks efficiently
- Assign and set in progress during initial actions for clear ownership
- Identify affected entity, flagged action, surrounding events, and validate with threat intelligence in investigation
- Assign true/false positive verdict, document steps and reasoning, then close in final actions

---
