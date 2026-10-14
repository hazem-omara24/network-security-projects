# Security Monitoring with Wazuh SIEM

A small SIEM lab built on Wazuh to collect, search and analyze security alerts from two endpoints.

## Environment
- Wazuh server (manager, indexer, dashboard)
- Agent 1: Kali Linux (`kali-agent`)
- Agent 2: Windows 10 host (`myHost`)
- Isolated VMware network 192.168.138.0/24

## What was done
- Registered both endpoints as Wazuh agents and verified they report as Active
- Generated failed authentication attempts and investigated them in Threat Hunting using `rule.groups: authentication_failed`
- Extracted source IP, targeted user, rule ID, severity and MITRE ATT&CK mapping for each alert
- Reviewed File Integrity Monitoring (FIM) alerts from the Windows host

## Example alert analysis

| Field | Value |
|-------|-------|
| Rule | 597, level 5: Registry Key Entry Deleted |
| Agent | myHost (Windows) |
| Detail | Deletion of a `cbdhsvc_<suffix>` service key (Clipboard User Service) |
| Mode | Scheduled FIM scan |
| MITRE tags | T1070.004, T1485, T1112 |
| Assessment | Likely false positive: this per-session service key is recreated and removed by Windows during normal logon and logoff |
| Action | Review context; tune FIM scope if the volume is high |

## Findings
- Wazuh recorded `sudo` usage on the Kali agent (Rule 5402) with the full command line, which makes tool usage on a monitored host traceable. See project 3.
- With default rules, no alert was generated on the Windows target for port scanning against it.

## Recommendations
- Add custom detection rules or collect firewall logs for port-scan detection
- Fail2ban and an account lockout policy against SSH brute force
- Tune FIM paths to reduce registry noise

## Files
- `screenshots/`: agents view, Threat Hunting results, alert detail, dashboard
