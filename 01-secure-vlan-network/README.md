# Secure Enterprise Network with VLANs and ACLs

Design and hardening of a small enterprise network in Cisco Packet Tracer.

## What it does
- Splits the network into three VLANs: Management (10), Employees (20), Guests (30)
- Router-on-a-stick inter-VLAN routing and a DHCP pool per VLAN
- Extended ACLs: guests cannot reach internal VLANs; employees cannot reach Management
- Port Security (sticky MAC, max 2, violation restrict) on user ports
- SSH v2 instead of Telnet; unused ports shut down

## Topology
R1 (g0/0) -- trunk -- SW1 (fa0/1); PC-Admin on fa0/2, PC-Staff on fa0/3, PC-Guest on fa0/4.

| VLAN | Name | Subnet | Gateway |
|------|------|--------|---------|
| 10 | Management | 192.168.10.0/24 | 192.168.10.1 |
| 20 | Employees | 192.168.20.0/24 | 192.168.20.1 |
| 30 | Guests | 192.168.30.0/24 | 192.168.30.1 |

## Files
- `configs/R1.txt`, `configs/SW1.txt`: device configurations (passwords replaced with placeholders)
- `screenshots/`: topology and verification output
- `project1.pkt`: Packet Tracer file (add yours here)

## Verification
- Staff to Guest: ping succeeds; Staff to Management: blocked
- Guest to Management/Employees: unreachable
- Admin to SW1: `ssh -l admin 192.168.10.2` succeeds
- `show access-lists`, `show port-security interface fa0/3`, `show ip dhcp binding`
