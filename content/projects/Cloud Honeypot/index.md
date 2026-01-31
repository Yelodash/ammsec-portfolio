---
title: Azure Cloud Honeypot & Threat Analysis
date: 2025-07-09
summary: Deployed a vulnerable Azure Virtual Machine to simulate a honeypot, aggregating and analyzing live brute-force attacks from the open internet.
tags:
  - Cloud
  - Forensics
tech_stack:
  - Microsoft Azure
  - Azure Virtual Machines
  - Network Security Groups (NSG)
  - Windows
  - RDP
  - Windows Event Viewer
  - Kali Linux
  - NetExec
featured: true
status: Live
role:
duration: 3 weeks
team_size: 1
highlights:
---

### Project Overview

In this project, I deployed a purposely vulnerable "Honeypot" in Microsoft Azure to monitor unauthorized access attempts and brute-force campaigns. The objective was to observe real-world attack vectors, analyse the velocity of automated botnets, and practice host-based forensics using Windows Event Logs.

---
## 1. Infrastructure Deployment

I provisioned a **Windows 10 Virtual Machine** within the Azure ecosystem. To ensure the instance acted as an effective target, I intentionally configured the environment to be non-compliant with standard hardening practices:

* **Host Type:** Azure Standard B-Series VM. 
* **Operating System:** Windows 10 Pro. 
* **Network Posture:** Publicly addressable IP with no edge filtering.
<img src="Images/image1.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">

---

## 2. Security Configuration (The "Bait")

**External Network Layer:** I modified the Azure **Network Security Group (NSG)** to expose the machine to the open internet. I created a custom inbound rule (`ALL_INBOUND_TRAFFIC`) allowing ingress traffic from **Any Source** on **Any Port**, effectively removing the cloud-layer firewall.
<img src="Images/image2.png" style="display: block; margin-left: auto; margin-right: auto; margin-bottom: 5px;">
**Internal Host Layer:** After provisioning, I established an administrative session via RDP to configure internal security policies. Using the Windows Firewall console (`wf.msc`), I systematically disabled all firewall profiles (Domain, Private, and Public) to ensure ICMP echo requests and RDP traffic could reach the kernel.

<img src="Images/image3.png" style="margin-bottom: 5px;">

<img src="Images/image4.png" style="margin-bottom: 5px;">

---
## 3. Attack Simulation & Validation

To validate the monitoring pipeline before exposing the host to live internet traffic, I executed a controlled brute-force attack against the honeypot using **Kali Linux**. I utilized `NetExec` (formerly CrackMapExec) to simulate a credential stuffing attack against the RDP service (Port 3389).

``` bash
netexec rdp 20.199.11.62  -u 'root' -p 'root'
```

<img src="Images/image5.png" style="margin-bottom: 5px;">

**Result:** The attack successfully triggered a `STATUS_LOGON_FAILURE` response, confirming that the authentication request bypassed the network perimeters and interacted with the internal OS authentication mechanism.

---
## 4. Log Analysis & Detection

Inside the VM, I utilized **Windows Event Viewer** to conduct host-based analysis. The brute-force attempts generated specific forensic artifacts:

- **Event ID:** 4625 (An account failed to log on).
- **Log Source:** Security.

<img src="Images/image6.png" style="margin-bottom: 5px;">

---
### Critical Analysis: Production vs. Lab

While this project successfully demonstrated the mechanics of a honeypot, a production-grade deployment would require significant architectural changes to meet GRC standards:

1. **Log Forwarding:** In a real enterprise environment, local logs (Event 4625) are volatile. They must be forwarded to a centralized SIEM (like **Azure Sentinel** or **Splunk**) to ensure data immutability and correlation.
2. **Network Isolation:** This VM was placed in a default subnet. A production honeypot must be strictly isolated in a DMZ or a separate Virtual Network (VNet) to prevent **lateral movement** if an attacker successfully compromises the machine.
3. **Emulation vs. Interaction:** This was a "High Interaction" honeypot (a real OS). Production environments often use emulation software (like **T-Pot** or **Cowrie**) to simulate services, reducing the risk of providing an attacker with a fully functional computing resource.

---
### Key Outcomes

- **Cloud Infrastructure Management:** Successfully deployed and configured a custom Azure Virtual Machine and manipulated Network Security Groups (NSGs) to create a controlled vulnerability environment.
- **Red Team Simulation:** Gained hands-on experience with offensive security tools (**Kali Linux**, **NetExec**) to simulate real-world brute-force attacks and validate network reachability.
- **Blue Team Log Analysis:** Developed proficiency in Windows Event Forensics, specifically identifying **Event ID 4625** to correlate network traffic with authentication failures.
- **Attack Chain Visualization:** Validated the complete attack lifecycle, monitoring how external threats traverse cloud firewalls to impact internal operating system logs.