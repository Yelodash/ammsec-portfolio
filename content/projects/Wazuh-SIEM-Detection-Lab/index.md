---
title: "SIEM Implementation: Wazuh Threat Detection Lab"
date: 2025-02-03
summary: Deployed a localized SOC environment using Wazuh to detect brute-force attacks and monitor endpoint security events
tags:
  - Forensics
tech_stack:
  - Wazuh SIEM
  - Linux VM
  - Windows endpoint
  - PowerShell
  - Log analysis
featured: true
status: Live
role: Solo Developer
duration: 3 weeks
team_size: 1
highlights:
  - PWA with offline support
  - 5000+ monthly active users
  - "Lighthouse score: 100"
---
### Project Overview

In this project, I deployed the **Wazuh Security Information and Event Management (SIEM)** platform to engineer a localized Security Operations Center (SOC). The primary objective was to establish centralized log aggregation, gain deep visibility into endpoint activities, and validate the system's ability to detect and correlate common attack vectors in real-time.

---
## 1. Infrastructure Architecture

I hosted the **Wazuh Manager** on a Linux-based virtual machine to serve as the central correlation engine and log aggregator.

* **Manager Node:** Linux (VirtualBox) hosting the analysis engine and Filebeat.
* **Interface:** Accessed the Wazuh Dashboard (Kibana) via the host browser to visualize security events.

<img src="Images/image1.png" style="margin-bottom: 5px;">

---
## 2. Endpoint Agent Configuration

I deployed the **Wazuh Agent** on a Windows 11 endpoint to capture system telemetry (Event Logs, File Integrity Monitoring). I utilized PowerShell automation to download the MSI installer and configure the agent to forward logs to the Manager's IP address.

```powershell
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.2-1.msi -OutFile $env:tmp\wazuh-agent; msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER='<IP ADDRESS>
```

``` powershell
NET START Wazuh
```

<img src="Images/image2.png" style="margin-bottom: 5px;">

---
## 3. Service Verification & Handshake

Post-installation, I verified the **Active** status of the agent to ensure the encrypted channel between the Endpoint and the Manager was successfully established.

<img src="Images/image3.png" style="margin-bottom: 5px;">
<img src="Images/image4.png" style="margin-bottom: 5px;">

---
## 4. Attack Simulation

To validate the detection rules, I executed a manual **brute-force attack** against the Windows endpoint, generating multiple consecutive login failures.

**Telemetry Analysis:** Navigating to the "Security Events" dashboard, I observed an immediate spike in the **"Authentication failures"** metric. This confirmed that the endpoint was correctly parsing local Windows Event Logs and forwarding them to the SIEM in near real-time.

<img src="Images/image5.png" style="margin-bottom: 5px;">

---
## 5. Forensic Event Drill-Down

A deep dive into the specific security alerts revealed the granularity of the data captured.

<img src="Images/image6.png" style="margin-bottom: 5px;">

---
### Key Outcomes

- **SOC Implementation:** Successfully established a functional threat detection pipeline, confirming active agent status and secure log ingestion.
- **Incident Response:** Validated the alert threshold logic by detecting a brute-force attempt immediately, triggering a high-severity notification.
- **Threat Visualization:** Demonstrated the ability to use SIEM dashboards to reconstruct an attack timeline and identify specific threat vectors.




