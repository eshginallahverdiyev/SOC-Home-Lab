# 🛡️ SOC Automation & Threat Detection Lab

## 📌 Project Overview
This project demonstrates the design and implementation of a home-based Security Operations Center (SOC) lab. The environment simulates real-world cyber attack scenarios and enables threat detection, log analysis, and incident response using **Wazuh SIEM/XDR**, **pfSense Firewall**, and endpoint telemetry.

---

## 🏗️ Lab Architecture
- **SIEM/XDR:** Wazuh Manager (Ubuntu-based OVA)
- **Firewall/Gateway:** pfSense
- **Victim Machine:** Windows 11 Enterprise
- **Attacker Machine:** Kali Linux
- **Network:** Isolated Virtual LAN (VMnet2)


## ⚙️ Resource Allocation:

| Virtual Machine | OS | RAM | Storage |
| :--- | :--- | :--- | :--- |
| **Wazuh Server** | Ubuntu | 3 GB | 50 GB |
| **pfSense** | FreeBSD | 512 MB | 20 GB |
| **Windows Victim**| Windows 11 | 2 GB | 60 GB |

---

## 🛠️ Step 1: Virtual Infrastructure & Resource Optimization

![Wazuh Resources](images/wazuh_settings.png)  
*Wazuh VM resource allocation.*

![Kali Settings](images/kali_settings.png)  
*Kali Linux VM configuration.*

![pfSense Settings](images/pfsense_settings.png)  
*pfSense VM configuration.*

---

## 🛡️ Step 2: pfSense Installation & Network Configuration

I configured **pfSense** as the gateway for the lab. It manages two network interfaces:
1. **WAN (NAT):** Provides internet access for updates.
2. **LAN (VMnet2):** An isolated network where the Victim and SIEM reside.

![pfSense Setup](images/pfsense_setup.png)  
*Initial pfSense installation.*

![Partitioning](images/pfsense_partitioning.png)  
![Partition Scheme](images/pfsense_partition_scheme.png)  
![Partition Editor](images/pfsense_partition_editor.png)  
![Partition Final](images/pfsense_partition.png)  
*Disk partitioning process.*

![Interfaces](images/pfsense_interfaces.png)  
*WAN/LAN interface configuration.*

![Dashboard](images/pfsense_web_dashboard.png)  
*pfSense web dashboard.*

---

## 💻 Step 3: Windows 11 Victim Deployment

To deploy Windows 11 in a resource-constrained environment, I bypassed the RAM and TPM requirements using Registry Editor during the installation phase.

![Installing](images/win11_installing.png)  
*Windows installation process.*

![Bypass](images/win11_bypass_requirements.png)  
*Bypassing system requirements.*

![Internet Check](images/win11_internet_check.png)  
*Network verification.*

![Provisioning](images/win11_provisioning.png)  
*System setup completion.*

![Settings](images/win11_settings.png)  
*Windows configuration.*

---

## 🔍 Step 4: Endpoint Telemetry (Sysmon + Wazuh Agent)

![Sysmon Install](images/win11_sysmon_install.png)  
*Sysmon installation.*

![Sysmon Config](images/wazuh_sysmon_config_done.png)  
*Sysmon configuration completed.*

![Agent Start](images/wazuh_agent_start.png)  
*Wazuh agent started.*

![Agent Import](images/wazuh_import.png)  
*Agent registration/import.*

---

## 🌐 Step 5: Network Connectivity Verification

![Ping pfSense](images/win11_ping_pfsense.jpg)  
*Connectivity between Windows and pfSense.*

---

## ⚔️ Step 6: Attack Simulation (Kali Linux)

![Nmap Attack](images/kali_nmap_attack.png)  
*Initial reconnaissance scan.*

![Full Scan](images/kali_nmap_full_scan.png)  
*Full port scan.*

---

## 🚨 Step 7: Detection & Alerting (Wazuh)

![Dashboard Status](images/wazuh_dashboard_status.png)  
*Wazuh system health.*

![Agents](images/wazuh_agents_preview.png)  
*Connected agents.*

![Alerts](images/wazuh_alerts_dashboard.png)  
*Generated security alerts.*

---

## 📊 Step 8: Log Analysis & Threat Hunting

![Event Logs](images/wazuh_event_logs_analysis.png)  
*Detailed log analysis.*

![Threat Hunting](images/wazuh_threat_hunting.png)  
*Threat hunting activity.*

---

## 🔐 Additional Configurations

![Firewall Disabled](images/pfsense_fw_disable.png)  
*Firewall rule testing.*

![Windows Firewall](images/win11_firewall_disable.png)  
*Windows firewall adjustments.*

---

## 🧠 Key Skills Demonstrated
- SIEM deployment & configuration (Wazuh)
- Firewall & network segmentation (pfSense)
- Endpoint telemetry (Sysmon, Wazuh Agent)
- Network reconnaissance detection (Nmap)
- Log analysis & alert triage
- Threat hunting

---

## 🚀 Conclusion
This lab demonstrates a complete SOC workflow: from infrastructure setup to attack simulation, detection, and investigation. It reflects real-world blue team operations in a controlled environment.
