# 🛠️ Phase 2: Exploitation, Persistence & SIEM Detection Analysis

This document covers the advanced phase of the SOC Lab, focusing on privilege escalation, persistence mechanisms, and how **Wazuh SIEM** detects these sophisticated attack techniques.

---

## ⚔️ Attack Lifecycle: From Footprint to Domain Dominance

### 1. Lateral Movement & Initial Shell
After identifying valid credentials (`Victim:Victim1234`) via brute-force, **Impacket's WmiExec** was utilized to gain an interactive shell. Unlike PsExec, WmiExec is stealthier as it does not create a new service.

![Impacket WmiExec](images/kali_impacketwmiexec.png)
*Analyst Note: WMI execution is a common technique for lateral movement (MITRE T1047).*

### 2. Privilege Escalation (UAC Bypass)
The initial shell had limited privileges. To gain full control, I leveraged the `fodhelper` UAC bypass vulnerability. This allowed me to elevate from a standard local administrator to **NT AUTHORITY\SYSTEM**.

![Metasploit Payload](images/kali_msfvenom_payload.png)
![UAC Bypass & GetSystem](images/meterpreter_bypassauc_getsystem.png)
*Technique: Using `exploit/windows/local/bypassuac_fodhelper` to achieve highest-level privileges.*

### 3. Persistence: Backdoor Accounts & Registry Keys
To ensure access survives a reboot, two persistence methods were implemented:
1. **Registry Run Key:** Setting a value in `HKCU\...\Run` to execute the payload on login.
2. **Hidden Admin Account:** Creating a backdoor user `Support_Acc` and hiding it from the Windows welcome screen.

![Persistence & Migration](images/kali_meterpreter_migrate_to_explorerexe_and_regsetval.png)
![Adding Backdoor User](images/meterpreter_shell_adding_backdoor_user.png)

---

## 🔍 Wazuh SIEM Detection Analysis

The primary goal of this lab is to validate detection rules. Below is the breakdown of how Wazuh alerted on the attack chain.

### 🚨 Alert 1: Credential Access (Brute Force)
Wazuh detected hundreds of failed authentication attempts followed by a success. This triggered a high-severity alert for RDP/SMB brute-forcing.

![Wazuh Logon Failures](images/wazuh_logon_failures.png)
![Attacker IP Tracking](images/wazuh_attacker_ip.png)

### 🚨 Alert 2: Privilege Escalation (Sysmon Telemetry)
Sysmon Event ID 1 (Process Creation) captured the anomaly when `fodhelper.exe` spawned a shell with elevated integrity.

![Wazuh Threat Hunting](images/wazuh_threat_hunting_dashboard.png)
*Detection Logic: Monitoring for suspicious parent processes of cmd.exe or powershell.exe.*

### 🚨 Alert 3: Persistence (Registry Modification)
Wazuh’s File Integrity Monitoring (FIM) and Sysmon Event ID 13 flagged the unauthorized modification of the "Run" key.

![Registry Change Detection](images/wazuh_registry_changes.png)
![Rule Description](images/wazuh_rule_description.png)

---

## 📂 Artifacts Captured
- **Credential Dumps:** Successful extraction of NTLM hashes via `secretsdump`.
- **Active Processes:** Monitoring `explorer.exe` migration.
- **RDP Logs:** NTLM hash identification in RDP logs.

![Secretsdump](images/kali_impacket_secretsdump.png)
![RDP NTLM Logs](images/wazuh_rdp_logon_ntlmhash_logs.png)

---

## 🚀 Conclusion
Phase 2 successfully demonstrates that while individual attack steps may be subtle, the **correlation** of endpoint telemetry (Sysmon) and SIEM rules (Wazuh) provides high-fidelity detection.

[⬅️ Back to Phase 1: Infrastructure Setup](README.md)
