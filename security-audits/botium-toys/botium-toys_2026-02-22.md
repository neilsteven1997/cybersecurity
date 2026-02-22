# Botium Toys Internal IT Audit  
**Google Cybersecurity Professional Certificate – Course Project**  
Neil Steven | February 22, 2026 | Portfolio: [github.com/neilsteven1997/cybersecurity](https://github.com/neilsteven1997/cybersecurity/tree/main/security-audits/botium-toys)

## Executive Summary
>[!Note]
>_This scenario is based on a fictional company:_

Botium Toys is a small U.S. business that develops and sells toys. The business has a single physical location, which serves as their main office, a storefront, and warehouse for their products. However, Botium Toy’s online presence has grown, attracting customers in the U.S. and abroad. As a result, their information technology (IT) department is under increasing pressure to support their online market worldwide. 

The manager of the IT department has decided that an internal IT audit needs to be conducted. She's worried about maintaining compliance and business operations as the company grows without a clear plan. She believes an internal audit can help better secure the company’s infrastructure and help them identify and mitigate potential risks, threats, or vulnerabilities to critical assets. The manager is also interested in ensuring that they comply with regulations related to internally processing and accepting online payments and conducting business in the European Union (E.U.).   

The IT manager starts by implementing the National Institute of Standards and Technology Cybersecurity Framework (NIST CSF), establishing an audit scope and goals, listing assets currently managed by the IT department, and completing a risk assessment. The goal of the audit is to provide an overview of the risks and/or fines that the company might experience due to the current state of their security posture.

**Goal:** _Provide an overview of potential risks, fines, and mitigation recommendations._

## Audit Scope & Objectives
- Scope: Internal IT systems, assets managed by IT department, online payment processing, EU customer data handling.  
- Framework: NIST CSF (Identify, Protect, Detect, Respond, Recover).  
- Key focus areas: Asset inventory, risk assessment, controls & compliance checklist.

## Key Findings
(2–4 sentence high-level summary of your biggest risks / gaps from the checklist – e.g., “Multiple ‘No’ responses in payment card security and access control increase risk of data breach and PCI DSS non-compliance.”)

## Controls & Compliance Checklist

**Question:** _Select “yes” or “no” to answer the question: Does Botium Toys currently have this control in place?_

**Controls assessment checklist**

- ❌ Least Privilege
- ❌ Disaster recovery plans
- ✅ Password policies
- ❌ Separation of duties
- ✅ Firewall
- ❌ Intrusion detection system (IDS)
- ❌ Backups
- ✅ Antivirus software
- ✅ Manual monitoring, maintenance, and intervention for legacy systems
- ❌ Encryption
- ❌ Password management system
- ✅ Locks (offices, storefront, warehouse)
- ✅ Closed-circuit television (CCTV) surveillance
- ✅ Fire detection/prevention (fire alarm, sprinkler system, etc.)

---

**Compliance checklist**

**Question:** _Select “yes” or “no” to answer the question: Does Botium Toys currently adhere to this compliance best practice?_

**Payment Card Industry Data Security Standard (PCI DSS)**

- ❌ Only authorized users have access to customers’ credit card information. 
- ❌ Credit card information is stored, accepted, processed, and transmitted internally, in a secure environment.
- ❌ Implement data encryption procedures to better secure credit card transaction touchpoints and data. 
- ❌ Adopt secure password management policies.

**General Data Protection Regulation (GDPR)**

- ❌ E.U. customers’ data is kept private/secured.
- ✅ There is a plan in place to notify E.U. customers within 72 hours if their data is compromised/there is a breach.
- ✅ Ensure data is properly classified and inventoried.
- ✅ Enforce privacy policies, procedures, and processes to properly document and maintain data.

**System and Organizations Controls (SOC type 1, SOC type 2)**

- ❌ User access policies are established.
- ❌ Sensitive data (PII/SPII) is confidential/private.
- ✅ Data integrity ensures the data is consistent, complete, accurate, and has been validated.
- ✅ Data is available to individuals authorized to access it.

---

## Recommendations & Prioritized Action Plan

**Recommendations**

>[!Note]
>_Prioritize implementation in this order. The IT manager will prepare a remediation plan with resource needs and provide quarterly progress updates to leadership._

1. **Generate a disaster recovery plan**
- **Current state:** Employees are working on a system without a backup.
- **Business impact:** Huge risks of data corruption from natural disasters such as flood, earthquakes and forest fire. And risks of data deletion from cyberattackers.
- **Action:** Deliver mandatory annual training and quarterly phishing simulations, with tracking of completion and performance metrics. 
- **Timeline:** Should be completed within 3 days.
- **Owner:** IT Department.  
- **Benefit:** Preservation of company critical data.

2. **Implement Access Control**
- **Current state:** Every employee have the same privilege and access. 
- **Business impact:** Poses a significant risk of insider data theft, including unauthorized exfiltration of customer PII/SPII or debit/credit card information by employees or other authorized users, potentially leading to regulatory penalties, financial losses, and reputational harm.
- **Action:** Deliver mandatory meeting with IT Department and employees.
- **Timeline:** Should be completed within 3 days.
- **Owner:** IT Manager (with HR support).  
- **Benefit:** Reduces incidents caused by human error and the likelihood of data theft, enhancing overall resilience against phishing and insider threats.

3. **Implement Strong Password Policy**
- **Current state:** Implementation of nominal password policy. 
- **Business impact:** Accounts with weak passwords are prone to hacking. 
- **Action:** Use 20 randomized characters with special symbols, using a reliable password generator.
- **Timeline:** Within 14 days.
- **Owner:** IT Department (with HR Assistance).  
- **Benefit:** Safer accounts by making passwords harder to guess or crack.

4. **Replacement of Legacy Systems**
- **Current state:** Employees are using legacy systems, or old systems without new updates and community support. 
- **Business impact:** Lacking new important features such as Intrusion Detection System (IDS), Centralized Password Management System, Backup feature, and it is expensive to maintain. 
- **Action:** Deliver new functional and tested system.  
- **Timeline:** Within 90 days. 
- **Owner:** IT Department (with IT Manager support) 
- **Benefit:** Increase in team productivity, system stability, and generate more sales. Proper controls will be in place and fully compliant with U.S. and international regulations and standards. 

5. **Establish Ongoing Security Awareness Training**
- **Current state:** Limited employee training increases phishing susceptibility.  
- **Business impact:** Social engineering remains a primary attack vector.  
- **Action:** Deliver mandatory annual training and quarterly phishing simulations, with tracking of completion and performance metrics. 
- **Timeline:** Initial cycle within 60 days; ongoing program.  
- **Owner:** IT Department (with HR support).  
- **Benefit:** Reduces human-error incidents and strengthens overall team resilience.

---

## Self-Assessment Score
Completed controls & compliance checklist: 5/5  
Self-assessment: Passed (100% / 5–5 points)

## Artifacts
- Coursera activity: [https://coursera.org/learn/manage-security-risks/assignment-submission](https://www.coursera.org/learn/manage-security-risks/assignment-submission/TMBj8/portfolio-activity-conduct-a-security-audit/attempt)
- GitHub writeups portfolio: [github.com/neilsteven1997/cybersecurity](https://github.com/neilsteven1997/cybersecurity/tree/main/writeups)

---
Last updated: February 22, 2026
