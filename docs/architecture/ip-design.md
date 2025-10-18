# Network & IP Plan

## 1. Overview
This document defines the IP addressing design for the Virtual Enterprise Network.  
It ensures consistent network segmentation, predictable host addressing, and simplified management across all VLANs and subnets.

The IP design aligns with:
- The [Network Topology](network-topology.md)
- The [DNS & DHCP Configuration](dns-dhcp.md)
- The [Firewall Rules](firewall-rules.md)

---

## 2. Design Objectives
- Maintain clear separation between internal, DMZ, and management networks  
- Reserve logical address ranges for each VLAN and service type  
- Use static IPs for servers and infrastructure components  
- Delegate DHCP for clients where appropriate  
- Avoid overlap between routed subnets  

---

## 3. Addressing Schema

| VLAN | Purpose | Subnet | Gateway | DHCP Range | Static Range | Notes |
|------|----------|---------|----------|-------------|---------------|-------|
| 10 | **Servers** | 10.0.10.0/24 | 10.0.10.1 | 10.0.10.50–100 | 10.0.10.2–49 | Domain Controllers, File Server, Monitoring |
| 20 | **Workstations** | 10.0.20.0/24 | 10.0.20.1 | 10.0.20.50–200 | 10.0.20.2–49 | Windows & Ubuntu clients |
| 30 | **DMZ** | 10.0.30.0/24 | 10.0.30.1 | — | 10.0.30.2–20 | Web & reverse-proxy servers |
| 40 | **Management** | 10.0.40.0/24 | 10.0.40.1 | — | 10.0.40.2–20 | OPNsense, hypervisor, admin tools |
| 50 | **Backup** | 10.0.50.0/24 | 10.0.50.1 | — | 10.0.50.2–10 | Off-site replication or backup traffic |

---

## 4. Static IP Assignments

| Hostname | Role | VLAN | IP Address | Notes |
|-----------|------|-------|-------------|-------|
| `OPNsense` | Firewall & Router | 40 | 10.0.40.2 | WAN → DHCP, LAN → VLAN Routing |
| `DC1` | AD DS / DNS / DHCP | 10 | 10.0.10.2 | Primary Domain Controller |
| `DC2` | AD DS / DNS | 10 | 10.0.10.3 | Redundant DC |
| `FS1` | File Server (SMB) | 10 | 10.0.10.4 | Shared storage |
| `WEB1` | Web Server (DMZ) | 30 | 10.0.30.2 | NGINX / Apache host |
| `MON1` | Monitoring Server | 10 | 10.0.10.5 | Prometheus + Grafana |
| `UBU1` | Linux Client | 20 | DHCP | Domain-joined workstation |
| `WIN1` | Windows 11 Client | 20 | DHCP | Domain-joined workstation |

---

## 5. IP Allocation Guidelines
- **Servers:** Static IPs reserved below `.50` in each VLAN  
- **DHCP Pools:** `.50–.200` range per VLAN for dynamic clients  
- **Gateways:** Always `.1` in each VLAN for consistency  
- **Broadcast Address:** Highest in range (e.g. `10.0.10.255`)  
- **DNS Servers:** Primary → DC1 (10.0.10.2), Secondary → DC2 (10.0.10.3)  

---

## 6. DNS Integration
Each subnet is associated with a corresponding DNS zone (e.g., `corp.local`, `dmz.local`).  
Static A-records are assigned for all critical infrastructure hosts.  
DHCP dynamically registers hostnames for clients in the `corp.local` zone.

See [DNS & DHCP Configuration](dns-dhcp.md) for implementation details.

---

## 7. Change Control
Any modifications to the IP schema must be documented in:
- [Architecture Decisions (ADRs)](../adr/)
- [Change Management Log](../operations/change-log.md)

---

## 8. Revision History

| Version | Date | Author | Summary |
|----------|------|--------|----------|
| 1.0 | 2025-10-18 | Brennan Griffin | Initial IP design for Virtual Enterprise Network |

