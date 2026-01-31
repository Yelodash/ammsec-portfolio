---
title: Remediation Plan - CVE-2023-34362
date: 2025-11-03
summary: A comprehensive GRC remediation strategy addressing the MOVEit Transfer breach, including root cause analysis, risk quantification, and ISO 27001 control mapping.
tags:
  - GRC
tech_stack:
  - ISO 27001 / 27002
  - ISO 27005 (risk assessment)
  - NIST 800-30 & 800-53
  - CIS Controls v8
  - COBIT (governance)
  - CVSS scoring
  - MITRE ATT&CK
  - MITRE D3FEND
  - UK GDPR (Article 32)
  - SIEM & SOC basics
  - Third-party risk management
links:
  - type: github
    url: uploads/remediation.pdf
    label: View report file in PDF here
featured: true
status: Live
role: Student
duration: 4 months
team_size: 2
highlights:
---
### Executive Summary

In this scenario-based case study, I developed a comprehensive **Remediation Plan** addressing a supply chain compromise within a critical HR & Payroll provider. Modeled after the real-world **MOVEit Transfer vulnerability (CVE-2023-34362)**, this project simulates the post-incident response phase required by GRC teams.

The objective was to translate technical failure into a **business-focused strategic roadmap**, demonstrating the ability to manage risk, ensure regulatory compliance (UK GDPR), and restore operational resilience.

**Frameworks Applied:** ISO 27001, ISO 27005 (Risk Management), NIST CSF, CIS Controls v8.

---

## 1. Methodology & Approach

#### **Root Cause Analysis (RCA)**
I moved beyond the technical exploit to identify the organizational failures that allowed the breach to occur.
* **People:** Lack of security awareness regarding third-party file transfer risks.
* **Process:** Failure in the Vendor Risk Management (VRM) lifecycle and delayed patch management policies.
* **Technology:** Absence of network segmentation for critical assets and insufficient EDR coverage.

#### **Threat & Asset Modelling**
I mapped critical business assets (Payroll Databases, Identity Management Systems) against relevant threat actors using a structured matrix. This ensured that the most valuable data received the highest priority for protection.

#### **Risk Quantification**
Using **ISO 27005** methodologies and **CVSS** scoring, I assessed the inherent risk of unpatched systems versus the residual risk after control implementation. This quantitative approach allows stakeholders to see the "Return on Security Investment" (ROSI).

---

## 2. Strategic Control Implementation

I designed a remediation roadmap focusing on immediate stabilization and long-term governance.

| Domain | Control Implemented | KPI for Success |
| :--- | :--- | :--- |
| **Identity Security** | Enforced Phishing-Resistant MFA & RBAC | 100% MFA adoption rate for admin accounts. |
| **Vulnerability Mgmt** | Automated Patching Cycles (72h SLA for Critical) | <48h Mean Time to Remediate (MTTR). |
| **Supplier Assurance** | New Vendor Risk Assessment Framework | 100% of Tier-1 vendors audited annually. |
| **Monitoring** | SIEM Log Ingestion for File Transfer Activity | <1 hour detection time for anomalous data egress. |

---

## 3. Business Impact Analysis

Effective GRC requires speaking the language of the business. I analyzed the impact of the controls on operations:
* **Financial Risk:** Reduced potential regulatory fines (GDPR 4% turnover) by demonstrating "State of the Art" security measures.
* **Operational Resilience:** Transitioned from a "Fragile" state to a "Resilient" state by implementing redundant backups and tested Incident Response (IR) playbooks.
* **Compliance Alignment:** Ensured the new architecture aligns with **UK GDPR Article 32** (Security of Processing) and **ISO 27001 Annex A controls**.

---

### Key Competencies Demonstrated

* **Governance & Strategy:** Ability to author policy and remediation plans that align with business goals.
* **Risk Management:** Proficiency in translating technical vulnerabilities (CVEs) into business risk registers.
* **Regulatory Knowledge:** Application of GDPR and ISO standards in a practical, recovery-focused scenario.
* **Control Design:** Developing SMART (Specific, Measurable, Achievable, Relevant, Time-bound) security controls.

---

### Full Remediation Report

<div style="text-align: center; margin-top: 20px;">
    <object data="/uploads/remediation.pdf" type="application/pdf" width="100%" height="600px" style="border: 1px solid #ddd; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
        <p>Your browser does not support PDF previews.</p>
        <a href="/uploads/remediation.pdf" style="background-color: #007bff; color: white; padding: 10px 15px; text-decoration: none; border-radius: 5px; font-weight: bold;">Download PDF</a>
    </object>
</div>