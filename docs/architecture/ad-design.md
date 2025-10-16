# Active Directory Design

- **Domain:** lab.local
- **OUs:** `Workstations/Windows`, `Workstations/Linux`, `Servers`, `Users`, `Groups`, `Administrators`,  `Domain Administrators`
- **Groups:** `GG_FileShare_Engineering_Read`, `GG_FileShare_Engineering_RW`
- **Service Accounts:** `svc_backup`, `svc_monitoring`
- **Key GPOs:**
  - Baseline security (password/lockout, Windows Defender, firewall on)
  - Map network drives to `\\srv-01\shares\%username%`
  - NTP: DC as time source
