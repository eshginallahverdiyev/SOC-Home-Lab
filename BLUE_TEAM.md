# 🔵 Blue Team: Detection & Threat Hunting

Analysis of the attack lifecycle using **Wazuh SIEM/XDR** and **Sysmon**.

## 🚨 1. Real-Time Alerting
Wazuh immediately flagged the brute-force and post-exploitation activities.

- **RDP Brute Force:** ![Logon Failures](images/wazuh_logon_failures.png)
- **Attacker Attribution:** ![Attacker IP](images/wazuh_attacker_ip.png)
- **Registry Modification:** ![Registry Alert](images/wazuh_registry_changes.png)

## 🔍 2. Forensics & Log Analysis
Deep dive into endpoint telemetry.

- **System Health:** ![Dashboard Status](images/wazuh_dashboard_status.png) ![Agents](images/wazuh_agents_preview.png)
- **NTLM Analysis:** Analyzing RDP login hashes. ![NTLM Logs](images/wazuh_rdp_logon_ntlmhash_logs.png)
- **Process Activity:** ![Log Analysis](images/wazuh_event_logs_analysis.png)

## 🎯 3. Threat Hunting Dashboard
Mapping alerts to the MITRE ATT&CK framework for strategic analysis.
![Hunting Dashboard](images/wazuh_threat_hunting_dashboard.png)
![Rule Description](images/wazuh_rule_description.png)

## 🛠️ 4. Telemetry Configuration
Verification of Sysmon and Wazuh agent status.
- **Sysmon Config:** ![Sysmon Config](images/wazuh_sysmon_config_done.png)
- **Agent Start:** ![Agent Start](images/wazuh_agent_start.png)
