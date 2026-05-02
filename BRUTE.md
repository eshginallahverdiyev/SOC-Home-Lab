## ⚔️ Phase 1: Attack Simulation (The Kill Chain)

### 1. Initial Access: RDP & SMB Brute-Force
The attacker initiated a brute-force attack against the RDP and SMB services using **Hydra** and **Netexec**.

![Hydra RDP Attack](images/kali_hydra_rdp_attack.png)  
*Hydra attempting to crack RDP credentials. Notice the account lockout challenges.*

![Netexec Success](images/kali_nxc_command_execution.png)  
*Successful credential validation via SMB using Netexec. Victim1234 identified.*

### 2. Exploitation: Gaining a Footprint
Using the cracked credentials, the attacker established a remote shell using **Impacket's WmiExec** to bypass noisy service creations.

![Impacket WmiExec](images/kali_impacketwmiexec.png)  
*Establishing an interactive semi-shell via WMI.*

### 3. Privilege Escalation: Bypassing UAC to SYSTEM
The initial shell was limited. By leveraging Metasploit's `bypassuac_fodhelper` and `getsystem`, the attacker elevated privileges to the highest level.

![Msfvenom Payload](images/kali_msfvenom_payload.png)  
*Creating a custom x64 reverse-tcp payload.*

![Metasploit Shell](images/msfconsole_getting_meterpreter_shell.png)  
*Receiving the initial Meterpreter session.*

![PrivEsc Success](images/meterpreter_bypassauc_getsystem.png)  
*Bypassing UAC and achieving NT AUTHORITY\SYSTEM.*

### 4. Post-Exploitation: Persistence & Evasion
To maintain access, a backdoor user was created, added to the Administrators group, and hidden from the Windows login screen.

![Backdoor Creation](images/meterpreter_shell_adding_backdoor_user.png)  
*Adding 'Support_Acc' as a permanent backdoor.*

![Process Migration](images/kali_meterpreter_migrate_to_explorerexe_and_regsetval.png)  
*Migrating to explorer.exe and setting a Registry Run Key for persistence.*

---

## 🔍 Phase 2: Detection & Analysis (Wazuh SIEM)

### 🚨 Use Case: Detecting Brute-Force & Lateral Movement

Wazuh captured the entire sequence of events through Sysmon and Windows Event Logs.

![Logon Failures](images/wazuh_logon_failures.png)  
*Wazuh alerting on multiple RDP logon failures (T1110 - Brute Force).*

![Registry Changes](images/wazuh_registry_changes.png)  
*Detection of unauthorized Registry modifications for persistence (T1547.001).*

![Threat Hunting](images/wazuh_threat_hunting_dashboard.png)  
*Visualizing the attack path in the Threat Hunting dashboard.*

### 📊 Key Indicators of Compromise (IoCs)
- **Authentication:** Hundreds of failed NTLM/RDP login attempts from a single source IP (`192.168.1.102`).
- **Process:** Unusual parent-child relationship (e.g., `fodhelper.exe` spawning a shell).
- **Registry:** New value added to `HKLM\...\Run` pointing to a non-standard path (`C:\Users\Public`).

---

## 🧠 MITRE ATT&CK Mapping
- **T1110:** Brute Force (Credential Access)
- **T1047:** Windows Management Instrumentation (Execution)
- **T1548.002:** Bypass User Account Control (Privilege Escalation)
- **T1136.001:** Local Account (Persistence)
- **T1021.001:** Remote Desktop Protocol (Lateral Movement)
