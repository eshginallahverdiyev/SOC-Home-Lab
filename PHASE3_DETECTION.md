# 🔍 Phase 3: Detection Strategy & SIEM Analysis

How we detect the attack using **Wazuh** and **Sysmon**.

## 🚨 1. Brute Force Detection
Multiple failed RDP attempts were correlated to identify a brute-force attack originating from `192.168.1.102`.

![Logon Failures](images/wazuh_logon_failures.png)

## 🛠️ 2. Registry & Configuration Monitoring
Wazuh’s FIM (File Integrity Monitoring) and Sysmon Event ID 13 captured the persistence attempt.

- **Alert:** Registry Key Added (High Severity)
- **Key Path:** `...\Windows\CurrentVersion\Run\WindowsUpdater`

![Registry Modifications](images/wazuh_registry_changes.png)

## 🎯 3. Threat Hunting Dashboards
The **Threat Hunting** dashboard provides a macro view of all suspicious activities, including:
- Process creation (Meterpreter)
- Network connections to unusual ports (4444)
- User account creation (`Support_Acc`)

![Threat Hunting Dashboard](images/wazuh_threat_hunting_dashboard.png)
