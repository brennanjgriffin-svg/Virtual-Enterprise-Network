# Firewall Rules (OPNsense)

## LAN → DC (AD DS/DNS)
| Src  | Dst  | Proto | Ports         | Action | Purpose       |
|------|------|-------|---------------|--------|---------------|
| LAN  | dc-01| TCP/UDP | 53,88,135,389,445,464,636,3268-3269 | Allow | AD/GPO/DNS |

## LAN → srv-01 (Services)
| Src  | Dst   | Proto | Ports     | Action | Purpose          |
|------|-------|-------|-----------|--------|------------------|
| LAN  | srv-01| TCP   | 22,80,443 | Allow  | SSH/Web          |
| LAN  | srv-01| TCP   | 445,139   | Allow  | SMB File Shares  |

## LAN → mon-01 (Monitoring)
| Src | Dst    | Proto | Ports | Action | Purpose |
|-----|--------|-------|-------|--------|---------|
| Any | mon-01 | TCP   | 9090  | Allow  | Prometheus UI (ex) |

## Isolation
- Deny host-to-host by default; only allow necessary flows above.

## Outbound
- LAN → Any: Allow (NAT via WAN)
