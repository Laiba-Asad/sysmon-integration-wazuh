# Windows Enhanced Logging Playbook for Wazuh (Sysmon Integration)

This project provides a step-by-step playbook for enabling enhanced logging on Windows endpoints and integrating those logs into Wazuh for improved security visibility.

---

## Overview

The purpose of this playbook is to strengthen endpoint monitoring by collecting high-value telemetry from:

- Sysmon (process, network, and system activity)
- PowerShell logs
- Windows Security events
- Windows Defender logs

Enhanced logging allows better detection, investigation, and threat hunting within a SIEM environment.

---

## Scope

This playbook applies to:

- Windows 10 / Windows 11
- Windows Server environments
- Systems with Wazuh agent installed and connected to a Wazuh Manager

---

## Key Components

### Sysmon
- Captures detailed system activity
- Logs process creation, network connections, and more
- Configured using a tuned XML configuration (recommended: SwiftOnSecurity)

### PowerShell Logging
- Script Block Logging
- Module Logging
- Transcription

### Windows Security Logs
- Logon activity
- Process creation
- Policy changes

### Windows Defender Logs
- Security alerts and protection events

---

## Implementation Steps

### 1. Deploy Sysmon
- Download from Microsoft Sysinternals
- Install with configuration file:
Sysmon64.exe -accepteula -i sysmonconfig.xml
- Validate via Event Viewer (Sysmon Operational logs)

---

### 2. Enable Windows Logging

Configure using Group Policy:

- Enable PowerShell logging
- Enable auditing for:
- Process Creation
- Logon Events
- Policy Changes

---

### 3. Configure Wazuh Agent

Update `ossec.conf` to include:

- Sysmon logs
- PowerShell logs
- Windows Defender logs
- Security logs

Restart the agent:
Restart-Service wazuh

---

### 4. Validate Log Ingestion

Trigger test events:

- Launch applications (process creation)
- Run network commands (ping)
- Execute PowerShell scripts

Verify:

- Logs appear in Wazuh dashboard
- Events are visible in alerts.json
- Fields such as `provider_name: Sysmon` are searchable

---

## Expected Outcome

After implementation:

- Sysmon is actively logging system activity
- PowerShell and Security logs are enabled
- Wazuh collects and correlates all logs
- Events are visible in the dashboard for analysis

---

## Key Benefits

- Improved visibility into endpoint behavior
- Detection of suspicious process execution
- Monitoring of PowerShell-based attacks
- Better investigation capabilities for SOC analysts

---

## Troubleshooting

Common issues:

- Agent not sending logs → Check ossec.conf syntax
- No Sysmon logs → Verify Sysmon service is running
- Logs missing in dashboard → Check Wazuh indexer/Filebeat
- Excessive noise → Tune Sysmon configuration

---

## References

- Wazuh Documentation (Windows Logging)
- Microsoft Sysinternals – Sysmon
- SwiftOnSecurity Sysmon Configuration

---

## Conclusion

This playbook establishes a strong logging foundation for Windows endpoints. By combining Sysmon, PowerShell, and Security logs with Wazuh, it significantly improves detection capabilities and supports effective SOC operations.

---

## Author

Laiba Asad
CyberSecurity Engineer
