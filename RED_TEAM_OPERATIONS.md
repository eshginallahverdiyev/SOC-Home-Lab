# 🔴 Red Team Operations: Comprehensive Adversary Emulation Report

This document provides an exhaustive technical breakdown of the offensive operations conducted within the SOC Home Lab. The simulation follows the **Cyber Kill Chain** methodology and is mapped to specific **MITRE ATT&CK®** techniques to evaluate the detection capabilities of the Wazuh SIEM.

---

## 🔍 Phase 1: Reconnaissance & Advanced Enumeration
**Objective:** To identify the target's attack surface and discover exploitable services.

### 1.1 Network Discovery (T1595 - Active Scanning)
The campaign began with an aggressive **Nmap** scan to map the network topology and identify live hosts.
- **Tools used:** `Nmap`
- **Technique:** Stealth SYN Scan followed by Service Version Detection.
- **Execution:**
```bash
nmap -sS -A -p- 192.168.1.101
```
### 1.2 Service Analysis
The scan revealed critical open ports:
- **Port 3389/TCP (RDP):** Indicates the target is likely a Windows machine and open for remote administration.
- **Port 445/TCP (SMB):** A prime target for lateral movement and credential harvesting.
![Nmap Full](images/kali_nmap_full_scan.png)
*Analysis:* The presence of RDP without Network Level Authentication (NLA) enforcement was identified as a potential vector for brute-force attacks.

---

## 🔨 Phase 2: Initial Access (T1110 - Brute Force)
**Objective:** To gain a legitimate foothold on the system by cracking user credentials.

### 2.1 RDP Authentication Cracking
Using the information from Phase 1, I targeted the `Victim` account. I employed **Hydra** combined with the `rockyou.txt` dictionary, which contains millions of known passwords.
- **Execution:**
```bash
hydra -l Victim -P /usr/share/wordlists/rockyou.txt rdp://192.168.1.101 -t4 -f -I
```
![Hydra RDP](images/kali_hydra_rdp_attack.png)
*Outcome:* The password was successfully recovered. This confirmed that the target lacked an account lockout policy, allowing unlimited authentication attempts.

---

## 📈 Phase 3: Weaponization & Payload Delivery
**Objective:** To establish a persistent Command and Control (C2) channel.

### 3.1 Payload Generation (T1587 - Internal Weaponization)
I utilized **msfvenom** to craft a sophisticated x64-based Meterpreter reverse shell. The payload was encoded to bypass basic signature-based detection.
![MSFVenom](images/kali_msfvenom_payload.png)

### 3.2 Staging and Delivery (T1105 - Ingress Tool Transfer)
I initiated a local **Python3 HTTP server** on the attacker machine to serve the payload. On the victim machine, I executed a PowerShell one-liner to download the binary into a temporary directory.
- **Attacker Side:** `python3 -m http.server 80`
![Python Server](images/kali_pythonwebserver.png)
- **Victim Side:** Gaining the initial Meterpreter session.
![Meterpreter Shell](images/msfconsole_getting_meterpreter_shell.png)

---

## 🔝 Phase 4: Privilege Escalation (T1548 - Abuse Elevation Control Mechanism)
**Objective:** To elevate from a standard user to the highest system privileges.

### 4.1 UAC Bypass (Fodhelper Technique)
The initial session was restricted. I utilized the `bypassuac_fodhelper` exploit, which manipulates the Windows Registry to execute a high-integrity process without a UAC prompt.
![UAC Bypass](images/meterpreter_bypassauc_getsystem.png)

### 4.2 Reaching NT AUTHORITY\SYSTEM
After bypassing UAC, I executed the `getsystem` command, which uses named-pipe impersonation to grant **SYSTEM** level access.
![Get System](images/kali_meterpreter_getsystem.png)

---

## 🕵️ Phase 5: Post-Exploitation & Persistence
**Objective:** To ensure long-term access and hide malicious traces.

### 5.1 Process Migration (T1055 - Process Injection)
To ensure the session survives if the original payload is closed, I migrated the Meterpreter process into a stable system process: `explorer.exe`.
![Migration](images/kali_meterpreter_migrate_to_explorerexe_and_regsetval.png)

### 5.2 Registry Run Keys (T1547.001 - Boot or Logon Autostart Execution)
I established persistence by modifying the Registry. This ensures the reverse shell automatically reconnects whenever the user logs in.
- **Command:** `reg setval -k HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\Run -v "WindowsUpdater" -d "C:\path\to\shell.exe"`

### 5.3 Backdoor Account Creation (T1136.001 - Local Account)
To maintain an alternative entry point, I created a hidden administrative account.
```bash
net user Support_Acc Password1234!! /add
net localgroup administrators Support_Acc /add
```
![Backdoor](images/meterpreter_shell_adding_backdoor_user.png)
*Verification:* I successfully re-entered the system via RDP using the new backdoor credentials.
![RDP Backdoor](images/kali_xfreerdp_to_backdoor_user.png)

---

## 📂 Phase 6: Credential Access & Stealthy Execution
**Objective:** To harvest sensitive data and test lateral movement techniques.

### 6.1 Dumping the SAM Database (T1003.002 - Security Account Manager)
Using **Impacket's Secretsdump**, I remotely extracted the NTLM hashes for all local accounts.
![Secretsdump](images/kali_impacket_secretsdump.png)

### 6.2 Stealthy Remote Execution (WMI & NetExec)
I demonstrated lateral movement capabilities using **WMIExec** and **NetExec (NXC)**. These methods are fileless and significantly harder to detect than standard RDP.
- **WMIExec Execution:** ![WMIExec](images/kali_impacketwmiexec.png)
- **NetExec (NXC) Command:** ![NXC](images/kali_nxc_command_execution.png)

---

## 📊 Summary of Findings
1.  **Weak Authentication:** The target's lack of account lockout policies allowed for a successful brute-force attack.
2.  **Privilege Management:** Standard users could bypass UAC, leading to full system compromise.
3.  **Telemetry Gaps:** While the attack was successful, every step (Registry changes, process migration, NTLM dumping) left a digital footprint for the Blue Team to analyze in the next phase.
