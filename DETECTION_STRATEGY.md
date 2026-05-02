# 🔍 Phase 3: Detection Strategy & SIEM Analysis

This phase analyzes how the attack was detected and visualized within the **Wazuh SIEM/XDR** platform.

---

## 🚨 1. Brute Force Detection

The initial brute-force attempts on RDP were captured through Windows Event Logs and Sysmon telemetry.

![Logon Failures](images/wazuh_logon_failures.png)
*Wazuh alerting on multiple RDP logon failures from the attacker IP.*

![Attacker IP Identification](images/wazuh_attacker_ip.png)
*Mapping the source IP (192.168.1.102) to multiple security events.*

---

## 🛠️ 2. Registry & Configuration Monitoring

Unauthorized changes to the system registry (used for persistence) triggered high-severity alerts.

![Registry Modifications](images/wazuh_registry_changes.png)
*Alert triggered by the addition of the 'WindowsUpdater' key in the Run registry.*

![Rule Analysis](images/wazuh_rule_description.png)
*Detailed breakdown of the Wazuh rule used to detect registry hijacking.*

---

## 🎯 3. Threat Hunting Dashboards

The attack was visualized in real-time, allowing for a comprehensive view of the "Kill Chain".

![Threat Hunting Dashboard](images/wazuh_threat_hunting_dashboard.png)
*Comprehensive view of endpoint alerts and anomalies.*

![RDP NTLM Logs](images/wazuh_rdp_logon_ntlmhash_logs.png)
*Analysis of NTLM hashes captured during RDP login attempts.*

---

## 🏁 Conclusion

Through the integration of **Sysmon** and **Wazuh**, every step of the attack—from initial port scanning to privilege escalation—left a traceable digital footprint. This setup ensures that even "stealthy" techniques like UAC bypass and process migration are flagged for analyst review.
