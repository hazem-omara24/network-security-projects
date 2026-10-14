# Security Assessment with Nmap

Assessment of a Windows 10 host in an isolated VMware lab, comparing its exposure with the Windows firewall enabled and disabled.

## Method
1. Host discovery on 192.168.138.0/24
2. Service and OS detection with the firewall enabled, then disabled
3. Default vulnerability scripts
4. Firewall re-enabled immediately after testing

```
sudo nmap -sn 192.168.138.0/24 -oN discovery.txt
sudo nmap -Pn -sV -O 192.168.138.1 -oN scan_after.txt    # firewall on
sudo nmap -Pn -sV -O 192.168.138.1 -oN scan_before.txt   # firewall off
sudo nmap -Pn --script vuln 192.168.138.1 -oN vuln_scan.txt
```

## Results

| Port | Service | Firewall on | Firewall off | Severity |
|------|---------|-------------|--------------|----------|
| 135/tcp | MS RPC | Filtered | Open | Medium |
| 139/tcp | NetBIOS-SSN | Filtered | Open | Medium |
| 445/tcp | SMB | Filtered | Open | High |
| 5357/tcp | WSDAPI | Open | Open | Low |

- Enabling the firewall reduced open ports from 4 to 1.
- Vulnerability scripts: `smb-vuln-ms10-054` returned false; two other checks (MS10-061, CVE-2012-1182) failed with connection errors and are inconclusive.

## Recommendations
Keep the firewall enabled on all profiles, restrict inbound rules to what is needed, disable SMBv1 and NetBIOS where unused, apply security updates, and re-run the inconclusive checks.

## Detection in Wazuh
Wazuh logged the four Nmap commands run on the Kali agent (Rule 5402, sudo to root) with full command lines. No alert was generated on the scanned Windows host.

## Files
- `results/`: Nmap output
- `report/`: full assessment report (PDF, add yours here)
- `screenshots/`: scan output and Wazuh alerts
