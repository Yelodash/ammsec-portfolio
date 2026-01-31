---
title: Phishing analysis and detection
date: 2024-09-20
summary: Designed and implemented an automated phishing analysis and response workflow using SOAR principles to identify, analyse, and triage phishing indicators efficiently.
tags:
  - Cloud
tech_stack:
  - Tines
  - SOAR workflows
  - Webhooks
  - REST APIs
  - VirusTotal
  - JSON
  - Email alerting
  - type: github
    url: https://github.com/alexjohnson/taskflow
    label: Code
  - type: live
    url: https://taskflow-demo.example.com
    label: Demo
featured: true
status: Live
role:
duration: 2 months
team_size: 5
highlights:
---
### Project Overview

Phishing investigations are notoriously repetitive and contribute significantly to SOC analyst burnout. In this project, I utilized **Tines**, a Security Orchestration, Automation, and Response (SOAR) platform, to engineer an automated triage workflow. The objective was to eliminate manual reputation checks by autonomously analysing suspicious URLs against threat intelligence feeds, effectively reducing "alert fatigue."

---
## 1. Playbook Architecture

I designed an end-to-end automation playbook that standardizes the incident lifecycle: **Ingest $\rightarrow$ Analyze $\rightarrow$ Respond**. This logic ensures that every reported URL undergoes the same rigorous check without human intervention, ensuring consistent handling of potential threats.

<img src="Images/image1.png" style="margin-bottom: 5px;">

---
## 2. Ingestion & Event Normalization

The pipeline is initiated via a **Webhook** trigger, simulating a real-world integration with an email gateway (e.g., Microsoft 365 Defender) or an internal "Report Phishing" button.

* **Trigger Mechanism:** HTTP Webhook receiving a JSON payload.
* **Input Data:** Raw JSON containing the suspicious URL and reporter metadata.
* **Normalization:** The event is parsed and sanitized to extract the target domain or IP address for analysis.

---
## 3. Threat Intelligence Enrichment

The core decision engine leverages an API integration with **VirusTotal** to assess the reputation of the extracted indicator.

* **Mechanism:** The workflow executes a **GET** request to the VirusTotal v3 API.

* **Decision Logic:** The system evaluates the "Malicious" vote count returned by the API.

``` sml
 If Malicious votes > 0:* The incident is flagged as High Priority.
 If Malicious votes = 0:* The incident is classified as Benign/Safe.
```

**Schema Validation & Testing:** To validate the workflow logic prior to production deployment, I constructed a standardized JSON schema to simulate various threat scenarios (e.g., High Risk vs. Safe). This ensures the alerting stage correctly handles dynamic variables like **risk_score** and **verdict**.

```yaml
{
  "verdict": "Malicious",
  "risk_score": 90,
  "source": "VirusTotal",
  "url": "http://suspicious-bank-login.com"
}
```

<img src="Images/image2.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---
## 4. Automated Alerting

Upon detecting a confirmed threat, the workflow triggers a notification action to alert the security team. I configured an **Email Agent** to dispatch a formatted report, ensuring analysts receive immediate context without logging into the platform.

**Dynamic Template Configuration:** The email body uses dynamic fields (Liquid templating) to inject the specific URL, Risk Score, and Verdict generated during the analysis phase.
<img src="Images/image3.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

**Execution Verification:** When the "Send Security Alert" agent is triggered, the system processes the payload instantly, transforming raw data into a human-readable alert.
<img src="Images/image4.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">
<img src="Images/image5.png" style="margin-bottom: 5px;">

**Result:** A high-priority notification reaches the analyst's inbox within seconds of the trigger, containing all necessary context for immediate remediation.

---
## Key Outcomes

- **Operational Efficiency:** Reduced the Mean Time to Respond (MTTR) for URL analysis from **~5 minutes (manual)** to **<5 seconds (automated)**.
- **API Integration:** Successfully demonstrated the ability to bridge orchestration platforms (Tines) with external Threat Intelligence providers (VirusTotal) via REST APIs.
- **Standardization:** Eliminated human error in the lookup process by enforcing a consistent analysis procedure for every alert.

