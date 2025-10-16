# DNS & DHCP

## DHCP Authority
- **Primary:** OPNsense (LAN)
- **Reservations:** DC, Servers (static or DHCP reservations)

## DNS
- **Zone:** lab.local
- **Authoritative:** DC (AD-integrated)
- **Forwarders:** OPNsense → public

## Records
| Name           | Type | Target/IP |
|----------------|------|-----------|
| dc-01.lab.local| A    | 10.0.0.10 |
| srv-01.lab.local| A   | 10.0.0.20 |
| mon-01.lab.local| A   | 10.0.0.30 |
| files.lab.local| CNAME| srv-01    |
| www.lab.local  | CNAME| srv-01    |
