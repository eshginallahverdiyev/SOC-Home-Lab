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
![Partition](images/pfsense_partition.png)  
![Partition Editor](images/pfsense_partition_editor.png)  
![Partition Scheme](images/pfsense_partition_scheme.png) 
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

## 🎯 Use Case: Detecting Nmap Network Scan

### 📖 Scenario
An attacker (Kali Linux) performs network reconnaissance using Nmap to identify open ports and services on the target machine.
This type of activity is commonly observed during the reconnaissance phase of an attack.

### 🎯 Objective
Detect and analyze reconnaissance activity using Wazuh SIEM and endpoint telemetry.

### 🧰 Data Sources
- Sysmon (Process Creation, Network Connections)
- Wazuh Agent Logs
- pfSense Network Traffic

### ⚠️ Indicators of Activity
- Multiple connection attempts to different ports
- Sequential scanning behavior
- Unusual network activity from a single source

### ✅ Outcome

The attack was successfully detected by Wazuh. Alerts were generated based on abnormal network activity and correlated with endpoint telemetry, allowing identification of reconnaissance behavior.

---

## 🧠 MITRE ATT&CK Mapping

- **T1046** – Network Service Discovery  
- **T1018** – Remote System Discovery

---

## 🚨 Step 7: Detection & Alerting (Wazuh)

![Dashboard Status](images/wazuh_dashboard_status.png)  
*Wazuh system health.*

![Agents](images/wazuh_agents_preview.png)  
*Connected agents.*

![Alerts](images/wazuh_alerts_dashboard.png)  
*Generated security alerts.*

## 🔍 Detection Analysis

The simulated Nmap scan triggered multiple alerts in Wazuh.

### 📌 Key Observations:
- High number of connection attempts within a short time frame
- Detection of scanning patterns targeting multiple ports
- Alerts generated from Sysmon network telemetry

### ⚙️ Detection Logic:
- Wazuh rules identified abnormal network behavior
- Correlation between process execution (Nmap) and network activity
- Alerts mapped to reconnaissance techniques

### 🧾 Sample Detection Evidence

Example indicators observed in Wazuh logs:

- Multiple connection attempts from a single source IP
- Sysmon Event ID 3 (Network Connection)
- Process: nmap.exe / nmap scan activity
- Destination ports scanned sequentially

These events contributed to triggering reconnaissance-related alerts.

### 🧾 Sample Log (Sysmon)

<Event>
  <EventID>3</EventID>
  <Image>C:\Program Files (x86)\Nmap\nmap.exe</Image>
  <DestinationIp>192.168.1.10</DestinationIp>
  <DestinationPort>80</DestinationPort>
</Event>

---

## ⚖️ False Positive Consideration

Similar patterns could be generated by legitimate network scanning tools used by administrators. Additional context such as source host, frequency, and behavior patterns should be analyzed to reduce false positives.

---

## 📊 Step 8: Log Analysis & Threat Hunting

![Event Logs](images/wazuh_event_logs_analysis.png)  
*Detailed log analysis.*

![Threat Hunting](images/wazuh_threat_hunting.png)  
*Threat hunting activity.*

## ⏱️ Attack Timeline

1. Attacker initiated Nmap scan from Kali Linux
2. Target system received multiple connection attempts
3. Sysmon logged network activity
4. Wazuh agent forwarded logs to SIEM
5. Wazuh generated alerts 
6. Analyst reviewed alerts in Wazuh dashboard  
7. Correlated logs with Sysmon data  
8. Identified activity as network reconnaissance (Nmap scan)

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
- MITRE ATT&CK mapping & detection alignment

---

## 📚 Lessons Learned

- Endpoint telemetry (Sysmon) is critical for visibility
- Network segmentation reduces attack exposure
- SIEM requires proper tuning to detect meaningful threats
- Correlating logs improves detection accuracy

---
## 🚀 Conclusion
This lab demonstrates a complete SOC workflow: from infrastructure setup to attack simulation, detection, and investigation. It reflects real-world blue team operations in a controlled environment.
