# 🛡️ Full-Stack SOC Automation & Threat Detection Lab

## 📌 Project Overview
This project demonstrates the design and implementation of a professional Security Operations Center (SOC) lab. It covers everything from infrastructure setup to detecting advanced post-exploitation techniques using **Wazuh SIEM/XDR**, **pfSense**, and **Sysmon**.

---

## 🏗️ Lab Architecture
The lab is built on an isolated Virtual LAN (VMnet2) to simulate a real corporate environment.

- **SIEM/XDR:** Wazuh Manager (Ubuntu) - Centralized log analysis.
- **Firewall/Gateway:** pfSense - Network segmentation and traffic control.
- **Victim Machine:** Windows 11 Enterprise (Telemetry via Sysmon & Wazuh).
- **Attacker Machine:** Kali Linux - Penetration testing toolkit.


| Virtual Machine | OS | RAM | Storage | Roles |
| :--- | :--- | :--- | :--- | :--- |
| **Wazuh Server** | Ubuntu | 3 GB | 50 GB | SIEM/XDR, Alerting |
| **pfSense** | FreeBSD | 512 MB | 20 GB | Gateway, Firewall |
| **Windows Victim**| Windows 11 | 2 GB | 60 GB | Target, Log Source |
| **Kali Linux** | Debian | 2 GB | 40 GB | Attacker |

---

## 🚀 Lab Phases (Detailed Documentation)
The lab is documented in three deep-dive phases:

1.  **[Phase 1: Reconnaissance & Initial Access](PHASE1_RECON.md)**
    *   Network scanning (Nmap), RDP Brute-forcing (Hydra), and initial SIEM alerting.
2.  **[Phase 2: Advanced Exploitation & Persistence](PHASE2_EXPLOIT.md)**
    *   Metasploit payloads, UAC Bypass, Process Migration, and Backdoor creation.
3.  **[Phase 3: Detection Strategy & Hunting](PHASE3_DETECTION.md)**
    *   Wazuh Dashboard analysis, Registry monitoring, and NTLM log investigation.

---

## 🛠️ Infrastructure Setup Highlights
### pfSense Network Configuration
I configured pfSense with two interfaces:
- **WAN:** NAT for external updates.
- **LAN (VMnet2):** Internal 192.168.1.0/24 subnet for the lab components.

![pfSense Dashboard](images/pfsense_web_dashboard.png)

### Windows 11 Deployment
To bypass Windows 11 hardware restrictions (RAM/TPM) in the virtual environment, I used Registry Editor tweaks during installation.
![Bypass](images/win11_bypass_requirements.png)

---

## 🧠 Key Skills Demonstrated
- **SIEM/XDR:** Rule customization and alert triage.
- **Threat Hunting:** Identifying IoCs (Indicators of Compromise).
- **Red Teaming:** Weaponization and delivery of payloads.
- **Networking:** Firewall rule management and traffic analysis.

---
## 🚀 Conclusion
This lab reflects a complete SOC lifecycle: from deployment to detection and investigation. It is a testament to my skills in both offensive and defensive cybersecurity.
