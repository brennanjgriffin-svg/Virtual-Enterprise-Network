# IP Plan

## VirtualBox Networks
- **WAN (NAT):** Internet egress for OPNsense
- **LAN (Internal Network):** `LAB_LAN` – all lab hosts
- **Guest-Only (Optional/Isolated):** `GUEST_NET`

## Subnets & Gateways
| VLAN/Net | CIDR        | Gateway       | DHCP Range         | Notes                  |
|---------:|-------------|---------------|--------------------|------------------------|
| LAN      | 10.0.0.0/24 | 10.0.0.1 (FW) | 10.0.0.100–10.0.0.200 | OPNsense LAN interface |
| GUEST    | 10.0.20.0/24| 10.0.20.1     | 10.0.20.50–.150    | Meant to simuate isolated guest-only netowrk; wifi to be added later|

## Host Addressing
| Role                | Hostname        | IP        |
|--------------------|-----------------|-----------|
| Firewall           | fw-opn          | 10.0.0.1  |
| Domain Controller  | dc-01           | 10.0.0.10 |
| File/Web/Backup    | srv-01          | 10.0.0.20 |
| Monitoring         | mon-01          | 10.0.0.30 |
| Win Workstation    | win-ws01        | DHCP      |
| Ubuntu Workstation | lin-ws01        | DHCP      |
