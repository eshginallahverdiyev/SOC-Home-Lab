# 🏗️ Phase 1: Infrastructure Design & System Deployment Guide

This document provides a comprehensive, step-by-step guide to the architecture and setup of the SOC Home Lab. The goal was to build a resilient and visible environment using enterprise-grade tools like **pfSense**, **Wazuh**, and **Sysmon**.

---

## 🖥️ 1. Virtual Hardware & Resource Allocation

Proper resource management is critical for a stable lab environment. Each component was allocated specific resources to ensure performance during intensive security monitoring and attack simulations.


| Virtual Machine | OS | RAM | Storage | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **Wazuh Server** | Ubuntu | 3 GB | 50 GB | Centralized Logging & SIEM |
| **pfSense** | FreeBSD | 512 MB | 20 GB | Network Gateway & Firewall |
| **Windows Victim**| Windows 11 | 2 GB | 60 GB | Vulnerable Endpoint |
| **Kali Linux** | Kali 2024 | 2 GB | 40 GB | Offensive Security Testing |

### Configuration Snapshots:
- **SIEM Resource Management:** ![Wazuh Settings](images/wazuh_settings.png)
- **Offensive Node Setup:** ![Kali Settings](images/kali_settings.png)
- **Firewall Optimization:** ![pfSense Settings](images/pfsense_settings.png)
- **Lab Topology Overview:** ![Architecture](images/lab_architecture.png)

---

## 🌐 2. pfSense: Network Gateway & Security Hardening

The pfSense firewall acts as the core of the network, providing segmentation between the WAN (Internet) and the isolated LAN (Lab Network - VMnet2).

### A. Installation & Partitioning
The installation process involved a manual disk partitioning scheme to ensure data integrity and system stability.
- **Initial Boot:** ![pfSense Setup](images/pfsense_setup.png)
- **Manual Partitioning Flow:**  
  ![Partitioning](images/pfsense_partitioning.png)  
  ![Partition Scheme](images/pfsense_partition_scheme.png)  
  ![Partition Editor](images/pfsense_partition_editor.png)  
  ![Disk Confirmation](images/pfsense_partition.png)

### B. Interface & Rule Configuration
Post-installation, interfaces were mapped to segregate traffic.
- **WAN/LAN Mapping:** ![Interfaces](images/pfsense_interfaces.png)
- **Web Management Dashboard:** ![Web Dashboard](images/pfsense_web_dashboard.png)
- **Security Policy Testing:** During the setup phase, firewall rules were periodically adjusted to verify connectivity. ![pfSense FW Disable](images/pfsense_fw_disable.png)

---

## 💻 3. Windows 11 Victim: Deployment & Hardening Bypass

To deploy Windows 11 in a virtualized lab environment, several hardware requirements (TPM, RAM) were bypassed to optimize performance.

### A. OS Installation Steps
- **Deployment Start:** ![Installing Win11](images/win11_installing.png)
- **Registry Bypass:** Utilizing `Regedit` during setup to bypass system requirements. ![Bypass Requirements](images/win11_bypass_requirements.png)
- **System Provisioning:** ![Provisioning](images/win11_provisioning.png)
- **Final Configuration:** ![Settings](images/win11_settings.png)

### B. Network & Security Baselines
Initial connectivity was verified via ICMP to the pfSense gateway.
- **Internet Check:** ![Internet Check](images/win11_internet_check.png)
- **Connectivity Test:** ![Ping Test](images/win11_ping_pfsense.jpg)
- **Firewall Baseline:** The Windows Firewall was intentionally adjusted to simulate specific vulnerability windows. ![Win11 Firewall Disable](images/win11_firewall_disable.png)

---

## 🔍 4. SIEM & Telemetry Ingestion (Wazuh & Sysmon)

Visibility is the cornerstone of a SOC. We deployed Wazuh as our SIEM and Sysmon as our granular endpoint sensor.

### A. Wazuh Server Deployment
The Wazuh manager was imported and configured to listen for incoming telemetry from the lab network.
- **Server Import:** ![Wazuh Import](images/wazuh_import.png)
- **System Status:** Monitoring the health of the manager. ![Dashboard Status](images/wazuh_dashboard_status.png)

### B. Endpoint Telemetry Pipeline
On the victim machine, Sysmon was installed with a custom configuration to capture advanced artifacts (Process creation, Registry changes).
- **Sysmon Deployment:** ![Sysmon Install](images/win11_sysmon_install.png)
- **Configuration Validation:** ![Sysmon Config Done](images/wazuh_sysmon_config_done.png)
- **Agent Activation:** Connecting the Windows endpoint to the Wazuh Manager. ![Agent Start](images/wazuh_agent_start.png)

---

## 🏁 Infrastructure Completion Summary
At this stage, the lab is fully operational:
1. **Isolated Segment:** All VMs are communicating over the VMnet2 LAN.
2. **Telemetry Flow:** Wazuh is successfully receiving logs from the Windows agent and Sysmon.
3. **Gateway Protection:** pfSense is managing all egress/ingress traffic.

The infrastructure is now ready for **Attack Simulation and Threat Hunting** modules.
