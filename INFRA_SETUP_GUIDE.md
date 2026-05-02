# 🛠️ Infrastructure & Environment Setup Guide

This document describes the full SOC Home Lab infrastructure design and deployment process, including network segmentation, virtualization, telemetry setup, and system hardening.

---

# 🌐 1. Network Architecture (pfSense Core Firewall)

The lab is built around a **pfSense firewall**, acting as the central routing and segmentation layer between attacker and victim environments.

## 🔹 Network Design
- WAN Interface: External NAT connectivity
- LAN Interface: Isolated internal subnet (192.168.1.0/24)
- VM Network: Virtualized attack & victim environment

![pfSense Setup](images/pfsense_setup.png)

## 🔹 Interface Configuration
![Interfaces](images/pfsense_interfaces.png)

## 🔹 Firewall Dashboard
![Dashboard](images/pfsense_web_dashboard.png)

---

# 💾 2. pfSense Installation & Disk Configuration

The firewall was deployed in a clean VM environment with manual disk partitioning.

![Partition Setup](images/pfsense_partitioning.png)
![Partition Scheme](images/pfsense_partition_scheme.png)
![Partition Editor](images/pfsense_partition_editor.png)

---

# 🪟 3. Windows 11 Victim Deployment

A Windows 11 VM was deployed as the target system for attack simulation.

## 🔹 Installation Process
![Win11 Install](images/win11_installing.png)
![Provisioning](images/win11_provisioning.png)

## 🔹 System Bypass Configuration
To bypass hardware requirements (TPM/Secure Boot):
- Registry modification via `LabConfig`
- Setup bypass using CMD (Shift + F10)

![Bypass Requirements](images/win11_bypass_requirements.png)

---

# 🔗 4. Network Connectivity Verification

## 🔹 Internal Network Testing
- ICMP connectivity between Windows VM and pfSense verified

![Ping pfSense](images/win11_ping_pfsense.jpg)

## 🔹 Internet Access Validation
![Internet Check](images/win11_internet_check.png)

---

# 📡 5. Telemetry & Logging Architecture

To ensure full visibility, endpoint monitoring was implemented using:

## 🔹 Sysmon (Deep Telemetry)
- Process creation logging
- Network connection tracking
- Registry modification monitoring

![Sysmon Config](images/wazuh_sysmon_config_done.png)

## 🔹 Wazuh Agent Deployment
- Windows Event Log forwarding
- Sysmon log ingestion
- Real-time SIEM integration

![Agent Start](images/wazuh_agent_start.png)

---

# 📊 6. Security Monitoring Validation

## 🔹 Wazuh Dashboard Health Check
![Dashboard Status](images/wazuh_dashboard_status.png)

---

# 🧠 Summary

This infrastructure enables:
- Full network segmentation (Attacker ↔ Firewall ↔ Victim)
- Complete endpoint visibility via Sysmon + Wazuh
- Realistic enterprise-like SOC simulation environment
