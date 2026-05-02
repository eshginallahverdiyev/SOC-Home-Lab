# 🛡️ End-to-End SOC Automation & Threat Detection Lab

## 📌 Project Overview
This comprehensive project demonstrates the architecture, deployment, and operation of a modern Security Operations Center (SOC). It covers the full lifecycle: **Infrastructure Setup**, **Attack Simulation (Red Teaming)**, and **Detection & Analysis (Blue Teaming)** using Wazuh SIEM, pfSense, and Sysmon.

## 📊 Lab Architecture

![Architecture](images/lab_architecture.png)

---

## 🏗️ Phase 1: Virtual Infrastructure & Networking
The lab is hosted on an isolated Virtual LAN (VMnet2) to simulate an enterprise environment while ensuring safety.

### ⚙️ Resource Allocation & VM Settings

| Component | OS | Role | RAM | Disk |
| :--- | :--- | :--- | :--- | :--- |
| **Wazuh Manager** | Ubuntu | SIEM/XDR | 3 GB | 50 GB |
| **pfSense** | FreeBSD | Firewall/GW | 512 MB | 20 GB |
| **Windows Target Endpoint**| Windows 11 | Target | 2 GB | 60 GB |
| **Kali Linux** | Debian | Adversary | 2 GB | 40 GB |

**Infrastructure Screenshots:**
![Wazuh Settings](images/wazuh_settings.png) ![Kali Settings](images/kali_settings.png) ![pfSense Settings](images/pfsense_settings.png)

---

## 🌐 Phase 2: Gateway & Endpoint Deployment

### 1. pfSense Firewall Configuration
Configured as the core router with WAN (NAT) and LAN (VMnet2) interfaces.
- **Partitioning & Setup:** ![Partitioning](images/pfsense_partitioning.png) ![Setup](images/pfsense_setup.png)
- **Dashboard & Interfaces:** ![Interfaces](images/pfsense_interfaces.png) ![Dashboard](images/pfsense_web_dashboard.png)

### 2. Windows 11 Target Endpoint Hardening & Telemetry
Deployed Windows 11 by bypassing TPM/RAM requirements and installing monitoring agents.
- **Bypass Requirements:** ![Bypass](images/win11_bypass_requirements.png)
- **Connectivity:** ![Ping pfSense](images/win11_ping_pfsense.jpg)
- **Telemetry Setup:**
    - **Sysmon:** Installed for granular logging. ![Sysmon Install](images/win11_sysmon_install.png)
    - **Wazuh Agent:** Forwarding logs to SIEM. ![Agent Start](images/wazuh_agent_start.png)

---

## ⚔️ Phase 3: Attack Simulation (The Red Team)

The attack follows the Cyber Kill Chain: Recon -> Access -> Privilege Escalation -> Persistence -> Credential Harvesting.

### 1. Reconnaissance & Discovery
- **Nmap Attack:** Initial service discovery. ![Nmap Attack](images/kali_nmap_attack.png)
- **Full Port Scan:** Identifying open RDP (3389) and SMB (445). ![Nmap Full](images/kali_nmap_full_scan.png)

### 2. Initial Access & Weaponization
- **Brute-Force:** Using **Hydra** to crack RDP credentials. ![Hydra](images/kali_hydra_rdp_attack.png)
- **Payload Generation:** Creating a reverse shell with **msfvenom**. ![MSFVenom](images/kali_msfvenom_payload.png)
- **Delivery:** Hosting the payload via Python web server. ![Python Server](images/kali_pythonwebserver.png)

### 3. Exploitation & Post-Exploitation
- **Meterpreter Session:** Gaining initial access. ![Meterpreter Shell](images/msfconsole_getting_meterpreter_shell.png)
- **Privilege Escalation:** Bypassing UAC to reach **SYSTEM** level. ![UAC Bypass](images/meterpreter_bypassauc_getsystem.png) ![GetSystem](images/kali_meterpreter_getsystem.png)
- **Remote Execution:** Using **Impacket-WMIExec** and **NetExec (NXC)** for command execution. ![WMIExec](images/kali_impacketwmiexec.png) ![NXC](images/kali_nxc_command_execution.png)

### 4. Persistence & Lateral Movement
- **Process Migration:** Moving to `explorer.exe` and setting registry run keys. ![Migration](images/kali_meterpreter_migrate_to_explorerexe_and_regsetval.png)
- **Backdoor Account:** Creating a hidden admin user. ![Backdoor](images/meterpreter_shell_adding_backdoor_user.png)
- **RDP Access:** Logging in as the backdoor user. ![RDP Backdoor](images/kali_xfreerdp_to_backdoor_user.png)

---
## 🎯 Attack vs Detection Mapping

| Attack Step | Tool | Detection Source | Alert | MITRE ID |
|------------|------|-----------------|-------|----------|
| Recon | Nmap | pfSense logs | Port scan | T1046 |
| Brute Force | Hydra | Event ID 4625 | Multiple failures | T1110 |
| Initial Access | RDP | Event ID 4624 | Suspicious login | T1078 |
| Privilege Escalation | Meterpreter | Sysmon Event ID 1 | New process | T1548 |
| Persistence | Registry | Sysmon Event ID 13 | Registry change | T1547 |
| Lateral Movement | WMIExec | Event ID 4688 | Remote execution | T1047 |

## 🛡️ Phase 4: Detection & Analysis (The Blue Team)

Wazuh SIEM provided full visibility into the attack lifecycle.

### 1. Alerting & Triage
- **Logon Failures:** Detecting the Hydra brute-force attack. ![Logon Failures](images/wazuh_logon_failures.png)
- **Attacker Attribution:** Identifying the Kali IP. ![Attacker IP](images/wazuh_attacker_ip.png)
- **Registry Monitoring:** Wazuh triggered a high-severity alert for the persistence key creation. ![Registry Alert](images/wazuh_registry_changes.png)

### 2. Threat Hunting & Forensics
- **Wazuh Overview:** General health of the SOC. ![Overview](images/wazuh_overwiew_dashboard.png)
- **Event Analysis:** Detailed investigation of process logs. ![Log Analysis](images/wazuh_event_logs_analysis.png)
- **NTLM Analysis:** Monitoring RDP login hashes. ![NTLM Logs](images/wazuh_rdp_logon_ntlmhash_logs.png)
- **Threat Hunting Dashboard:** Mapping all activities to the MITRE ATT&CK framework. ![Hunting Dashboard](images/wazuh_threat_hunting_dashboard.png)

## 🚨 Detection Engineering

### Brute Force Detection
- Data Source: Windows Security Logs (Event ID 4625)
- Logic: Multiple failed logons from a single IP
- Threshold: 5 attempts in 60 seconds
- MITRE: T1110 (Brute Force)

### Persistence Detection
- Data Source: Sysmon Event ID 13
- Logic: Monitor registry modifications in autorun locations
- Target Path:
  HKCU\Software\Microsoft\Windows\CurrentVersion\Run
- MITRE: T1547 (Boot or Logon Autostart Execution)

---

## 🧠 Key Skills & Tools
- **SIEM:** Wazuh (Agent/Manager), FIM, Custom Rules.
- **Red Teaming:** Metasploit, Impacket, Hydra, Nmap, NetExec.
- **Networking:** pfSense, VLAN Isolation, Firewall Rules.
- **Endpoint:** Windows Registry Analysis, Sysmon, Event Viewer.
- **Forensics:** Analyzing NTLM hashes and log correlation.

## 🏁 Conclusion
This lab demonstrates the effectiveness of centralized logging and endpoint telemetry in detecting multi-stage attacks. By correlating Sysmon and Windows logs within Wazuh SIEM, it was possible to identify, trace, and analyze each phase of the attack lifecycle. This highlights the importance of detection engineering and proactive threat hunting in modern SOC environments.

## 📄 Incident Summary

- Initial Access: RDP brute force
- Privilege Escalation: UAC bypass
- Persistence: Registry run key
- Credential Access: NTLM hash exposure via RDP logs

Impact:
- Unauthorized administrative access gained
- Persistence mechanism established
- Potential credential exposure via NTLM logs

### Recommendations
- Enable account lockout policy
- Restrict RDP access
- Monitor registry changes

### Custom Detection Rule (Wazuh)

```xml
<rule id="100001" level="10">
  <if_sid>4625</if_sid>
  <frequency>5</frequency>
  <timeframe>60</timeframe>
  <description>Possible RDP brute force attack</description>
</rule>
```
