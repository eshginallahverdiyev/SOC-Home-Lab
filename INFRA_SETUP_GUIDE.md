# 🛠️ Infrastructure & Environment Setup Guide

This document provides a step-by-step guide on how the SOC Home Lab environment was built from scratch.

## 1. Network Topology (pfSense)
The core of the lab is the **pfSense** firewall, which provides isolation and routing.

### Interface Configuration:
- **WAN:** Connected via NAT for external updates.
- **LAN:** Isolated VMnet2 network (192.168.1.0/24).

![pfSense Setup](images/pfsense_setup.png)
*Initial installation and disk partitioning process.*

![Interfaces](images/pfsense_interfaces.png)
*Final WAN/LAN configuration in pfSense console.*

---

## 2. Windows 11 Victim Deployment
To run Windows 11 in a virtualized lab, I performed a manual bypass of TPM and RAM requirements.

### Bypass Steps:
- Used `Shift + F10` during setup to open Command Prompt.
- Modified Registry (`HKEY_LOCAL_MACHINE\SYSTEM\Setup\LabConfig`).

![Bypass Requirements](images/win11_bypass_requirements.png)
![Provisioning](images/win11_provisioning.png)

---

## 3. Telemetry & Log Shipping
Visibility is key for a SOC. I deployed two critical agents on the victim:

- **Sysmon:** Installed with a modular configuration for deep process/network monitoring.
- **Wazuh Agent:** Configured to forward Windows Event Logs and Sysmon logs to the Wazuh Manager.

![Sysmon Config](images/wazuh_sysmon_config_done.png)
![Agent Start](images/wazuh_agent_start.png)

---

## 4. Connectivity Verification
Testing communication between the segments:
- **Windows to pfSense:** ![Ping pfSense](images/win11_ping_pfsense.jpg)
- **Wazuh Dashboard Status:** ![Dashboard Status](images/wazuh_dashboard_status.png)
