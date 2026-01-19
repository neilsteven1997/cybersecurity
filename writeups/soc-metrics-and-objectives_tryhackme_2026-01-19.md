# SOC Metrics and Objectives

---

SOC efficiency aligns with protecting organizational assets—confidentiality, integrity, availability—through alert development, 
intake, and triage. Level 1 analysts contribute by escalating true positives reliably to Level 2. Core indicators include 
alerts count as total received, measuring analyst load; false positive rate as false positives over total alerts, gauging 
noise levels; alert escalation rate as escalated over total, evaluating Level 1 autonomy; threat detection rate as detected 
over total threats, assessing team reliability.

Alerts count ideally sits between five and thirty per analyst daily, avoiding overload that masks threats or underload signaling
visibility gaps like SIEM failures. False positive rate at eighty percent or higher demands remediation through tool tuning, 
often termed false positive remediation, as excessive noise dulls vigilance. Alert escalation rate targets below twenty percent, 
balancing independence with necessary senior input. Threat detection rate must reach one hundred percent; lower values risk 
severe impacts like ransomware or exfiltration.

Service level agreement (SLA), a contract between SOC and management or managed security service provider (MSSP) and clients, 
sets response benchmarks. SOC team availability often spans twenty-four seven or standard business hours. Mean time to detect 
(MTTD) averages compromise to alert, aimed at five minutes. Mean time to acknowledge (MTTA) tracks alert to triage start, 
targeted at ten minutes. Mean time to respond (MTTR) spans detection to containment—device isolation or account securing—capped 
at sixty minutes.

These metrics drive performance reviews and advancement for Level 1 analysts. Improvements address specific shortfalls. I find 
tracking them personally useful for spotting workflow bottlenecks early.

---

| Description | Code/Command |
|-------------|--------------|
| None in source | N/A |

---

Extracted Tables

| Metric | Formula | Measures |
|--------|---------|----------|
| Alerts Count | AC = Total Count of Alerts Received | Overall load of SOC analysts |
| False Positive Rate | FPR = False Positives / Total Alerts | Level of noise in the alerts |
| Alert Escalation Rate | AER = Escalated Alerts / Total Alerts | Experience of L1 analysts |
| Threat Detection Rate | TDR = Detected Threats / Total Threats | Reliability of the SOC team |

---

| Metric | Common SLA | Description |
|--------|------------|-------------|
| SOC Team Availability | 24/7 | Working schedule of the SOC team, often Monday-Friday (8/5) or 24/7 mode |
| Mean Time to Detect (MTTD) | 5 minutes | Average time between the attack and its detection by SOC tools |
| Mean Time to Acknowledge (MTTA) | 10 minutes | Average time for L1 analysts to start triage of the new alert |
| Mean Time to Respond (MTTR) | 60 minutes | Average time taken by SOC to actually stop the breach from spreading |

---

| Issue | Recommendations |
|-------|-----------------|
| False Positive Rate over 80% | Your team receives too much noise in the alerts. Try to: 1. Exclude trusted activities like system updates from your EDR or SIEM detection rules 2. Consider automating alert triage for most common alerts using SOAR or custom scripts |
| Mean Time to Detect over 30 min | Your team detects a threat with a high delay. Try to: 1. Contact SOC engineers to make the detection rules run faster or with a higher rate 2. Check if SIEM logs are collected in real-time, without a 10-minute delay |
| Mean Time to Acknowledge over 30 min | L1 analysts start alert triage with a high delay. Try to: 1. Ensure the analysts are notified in real-time when a new alert appears 2. Try to evenly distribute alerts in the queue between the analysts on shift |
| Mean Time to Respond over 4 hours | SOC team can't stop the breach in time. Try to: 1. As L1, make everything possible to quickly escalate the threats to L2 2. Ensure your team has documented what to do during different attack scenarios |

---

### Key Takeaways
- Exclude trusted activities from detection rules to reduce false positives
- Automate triage for common alerts using SOAR or scripts
- Tune detection rules for faster execution to improve MTTD
- Verify real-time log collection to avoid ingestion delays
- Implement real-time notifications for new alerts to lower MTTA
- Distribute queue evenly among on-shift analysts
- Escalate threats promptly from Level 1 to Level 2 for better MTTR
- Document response actions for various scenarios

---
