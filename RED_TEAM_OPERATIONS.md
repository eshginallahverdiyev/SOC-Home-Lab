# 🔴 Red Team Operations: Adversary Emulation

This document details the exploitation phase of the lab, following the MITRE ATT&CK framework.

## 🔍 1. Reconnaissance (T1595)
The attack started with identifying open services on the target (`192.168.1.101`).
- **Initial Discovery:** ![Nmap Attack](images/kali_nmap_attack.png)
- **Service Scanning:** ![Nmap Full](images/kali_nmap_full_scan.png)
*Identified Services: RDP (3389) and SMB (445).*

## 🔨 2. Initial Access: Brute Force (T1110)
Used **Hydra** to gain access via RDP using common credential lists.
![Hydra RDP](images/kali_hydra_rdp_attack.png)

## 📈 3. Exploitation & Privilege Escalation (T1068)
### Weaponization
Generated a reverse shell payload and delivered it via a Python server.
![MSFVenom](images/kali_msfvenom_payload.png) ![Python Server](images/kali_pythonwebserver.png)

### Gaining a Foothold
Established a Meterpreter session: ![Meterpreter Shell](images/msfconsole_getting_meterpreter_shell.png)

### Escalating to SYSTEM
Used UAC bypass techniques to gain full administrative control.
![UAC Bypass](images/meterpreter_bypassauc_getsystem.png) ![Get System](images/kali_meterpreter_getsystem.png)

## 🕵️ 4. Persistence & Lateral Movement
### Process Migration & Registry Keys
Migrated the shell to `explorer.exe` for stability and added a 'Run' key for persistence.
![Migration](images/kali_meterpreter_migrate_to_explorerexe_and_regsetval.png)

### Backdoor Accounts
Created a hidden admin account called `Support_Acc`.
![Backdoor](images/meterpreter_shell_adding_backdoor_user.png)
- **Verifying Access:** ![RDP Backdoor](images/kali_xfreerdp_to_backdoor_user.png) ![RDP Windows](images/kali_xfreerdp_to_win11.png)

### Command Execution
Used **Impacket-WMIExec** and **NetExec (NXC)** for stealthy execution.
![WMIExec](images/kali_impacketwmiexec.png) ![NXC](images/kali_nxc_command_execution.png)

## 📂 5. Credential Access
Dumped local NTLM hashes from the SAM database.
![Secretsdump](images/kali_impacket_secretsdump.png)
