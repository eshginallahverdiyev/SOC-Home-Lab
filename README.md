# SOC-Home-Lab
SOC Home Lab for threat detection and incident response, featuring Wazuh (SIEM/XDR), pfSense (Firewall), Kali Linux (attacker), and Windows 11 Enterprise (victim).

# 🛡️ SOC Automation & Threat Detection Lab

## 📌 Project Overview
This project demonstrates the setup of a home-based Security Operations Center (SOC). I have built a virtualized environment to simulate real-world cyber attacks, monitor them via **Wazuh SIEM/XDR**, and implement defensive measures using **pfSense Firewall**.

## 🏗️ Lab Architecture
- **SIEM/XDR:** Wazuh Manager (Ubuntu-based OVA)
- **Firewall:** pfSense (FreeBSD-based)
- **Victim:** Windows 11 Enterprise
- **Attacker:** Kali Linux
- **Network:** Isolated Virtual LAN (VMnet2)

---

## 🛠️ Step 1: Virtual Infrastructure & Resource Optimization
Since I am working with **8GB RAM**, I optimized each Virtual Machine to ensure a smooth simulation environment.

### ⚙️ Resource Allocation:

| Virtual Machine | OS | RAM | Storage |
| :--- | :--- | :--- | :--- |
| **Wazuh Server** | Ubuntu | 3 GB | 50 GB |
| **pfSense** | FreeBSD | 512 MB | 20 GB |
| **Windows Victim**| Windows 11 | 2 GB | 60 GB |

![Wazuh Resources](images/wazuh_settings.png) 
*Caption: Configuring Wazuh with 3GB RAM and 2 CPU cores.*

---

## 🛡️ Step 2: Firewall & Network Segmentation
I configured **pfSense** as the gateway for the lab. It manages two network interfaces:
1. **WAN (NAT):** Provides internet access for updates.
2. **LAN (VMnet2):** An isolated network where the Victim and SIEM reside.

![pfSense Interfaces](images/pfsense_interfaces.png)
*Caption: pfSense dashboard showing WAN and LAN configurations.*

---

## 💻 Step 3: Victim Machine Setup (Windows 11)
To deploy Windows 11 in a resource-constrained environment, I bypassed the RAM and TPM requirements using Registry Editor during the installation phase.

![Windows Bypass](images/win11_bypass_requirements.png)
*Caption: Bypassing Windows 11 system requirements via Registry Editor (LabConfig).*

---

## 🚀 Next Steps (In Progress)
- [ ] Install **Wazuh Agent** on Windows 11.
- [ ] Configure **Sysmon** for advanced telemetry.
- [ ] Perform a **Brute Force** attack from Kali Linux.
- [ ] Analyze alerts in the **Wazuh Dashboard**.
