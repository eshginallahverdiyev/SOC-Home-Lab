# 🛡️ Phase 1: Reconnaissance & Brute-Force Attack

## 🔍 1. Network Reconnaissance (Nmap)
The attack started with a full port scan to identify open services on the victim machine (`192.168.1.101`).

![Nmap Full Scan](images/kali_nmap_full_scan.png)
*Nmap discovery showing open RDP (3389) and SMB (445) ports.*

## 🔨 2. Brute-Force Attack (Hydra)
After identifying the open RDP port, a brute-force attack was launched using the `rockyou.txt` wordlist.

![Hydra RDP Attack](images/kali_hydra_rdp_attack.png)
*Hydra attempting to crack the 'Victim' account password.*

## 🚨 3. SIEM Detection (Wazuh)
Wazuh immediately flagged the failed login attempts.

![Wazuh Logon Failures](images/wazuh_logon_failures.png)
*High-severity alert: Multiple failed login attempts (T1110).*

![Wazuh Attacker IP](images/wazuh_attacker_ip.png)
*Identifying the source of the attack in Wazuh dashboard.*
