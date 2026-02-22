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

**Question:** _Then, select “yes” or “no” to answer the question: Does Botium Toys currently adhere to this compliance best practice?
_

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
1. High priority: Enable MFA across all accounts (immediate).  
2. Medium: Conduct full asset inventory update and patch management review.  
3. Low: Document incident response plan.  

## Self-Assessment Score
Completed controls & compliance checklist: 5/5  
Self-assessment: Passed (≥80% / 4–5 points)

## Artifacts
- Full Coursera activity document: [Botium-Toys-Audit-Checklist.pdf](./Botium-Toys-Audit-Checklist.pdf)
- GitHub writeups portfolio: https://github.com/neilstevenplayz/cyber-writeups

---
Last updated: February 20, 2026
