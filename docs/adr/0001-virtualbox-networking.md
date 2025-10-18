# Architectural Decision Records — Virtual Enterprise Network

This document collects all major architectural decisions made for the Virtual Enterprise Network project. Serves as practive for documention and common practices. (New to formatting style). 

---

## Foundational Architecture

### ADR-0001: Virtualization of the Lab Environment
**Status:** Accepted  
**Context:** Physical hardware is expensive and inflexible. Virtualization enables isolation, snapshots, and portability.  
**Decision:** Use **VirtualBox** for all virtual machines.  
**Alternatives Considered:** VMware Workstation, Hyper-V, physical hardware.  
**Consequences:** Lower realism but excellent cost efficiency and portability. Single host allows for single point of failure and security vulnerability. Not ideal for enterprise application or data-sestive networks.

---

### ADR-0002: Network Segmentation Design
**Status:** Accepted  
**Context:** Segmentation improves security and organization.  
**Decision:** Create VLANs for Servers, DMZ, Workstations, and Management.  
**Alternatives Considered:** Flat topology.  
**Consequences:** Added complexity; improved control and scalability.

---

### ADR-0003: Firewall and Routing Platform
**Status:** Accepted  
**Context:** A firewall/router is required for NAT and VLAN routing.  
**Decision:** Use **OPNsense** for routing and security.  
**Alternatives Considered:** pfSense  
**Consequences:** Feature-rich open source platform with GUI.

---

### ADR-0004: IP Addressing & Subnet Scheme
**Status:** Accepted  
**Context:** Consistent IP management prevents overlap and confusion.  
**Decision:** Use private addressing (e.g., 10.0.x.x /24 per VLAN).  
**Alternatives Considered:** Single flat /24.  
**Consequences:** Requires planning; improves scalability.

---

### ADR-0005: Domain Architecture and Directory Services
**Status:** Accepted  
**Context:** Need centralized authentication and management.  
**Decision:** Deploy **Windows Server 2022 AD DS**.  
**Alternatives Considered:** standalone accounts.  
**Consequences:** Increases realism and enterprise alignment.

---

### ADR-0006: Redundant Domain Controller Design
**Status:** Accepted  
**Context:** Single DC creates a failure point.  
**Decision:** Add a secondary DC for DNS/DHCP redundancy.  
**Alternatives Considered:** One DC only.  
**Consequences:** More reliability, slight replication overhead.

---

### ADR-0007: Time Synchronization Strategy
**Status:** Accepted  
**Context:** Kerberos requires synchronized time.  
**Decision:** Use OPNsense or primary DC as NTP source.  
**Alternatives Considered:** External NTP per host.  
**Consequences:** Domain-wide consistency, internal dependency. 

---

## 💾 Core Services & Infrastructure

### ADR-0008: Centralized DNS and DHCP Design
**Status:** Accepted  
**Decision:** Host DNS and DHCP on domain controllers.  
**Alternatives:** OPNsense DHCP/DNS.  
**Consequences:** Simplifies domain joins and record management.

---

### ADR-0009: File Services Implementation
**Status:** Accepted  
**Decision:** Provide SMB file shares on Windows or Samba.  
**Alternatives:** NFS, SFTP.  
**Consequences:** Works natively with Windows; easy permissions.

---

### ADR-0010: Linux Domain Integration
**Status:** Accepted  
**Decision:** Integrate Ubuntu hosts using Samba + SSSD.  
**Alternatives:** Local accounts only.  
**Consequences:** Unified authentication across OSes.

---

### ADR-0011: Web Services and DMZ Policy
**Status:** Accepted  
**Decision:** Host web servers in a DMZ VLAN with restricted rules.  
**Alternatives:** Flat internal hosting.  
**Consequences:** Improves realism and security.

---

### ADR-0012: Backup and Recovery Strategy
**Status:** Accepted  
**Decision:** Use VirtualBox snapshots and rsync/Clonezilla images.  
**Alternatives:** Cloud backups, none.  
**Consequences:** Manual effort; strong rollback safety.

---

### ADR-0013: Monitoring and Observability Stack
**Status:** Accepted  
**Decision:** Deploy **Prometheus + Grafana** for system monitoring.  
**Alternatives:** Zabbix
**Consequences:** Modern dashboarding with minimal resources.

---

### ADR-0014: Remote Access Configuration
**Status:** Accepted  
**Decision:** Enable RDP for Windows and SSH for Linux; limit access by VLAN/firewall.  
**Alternatives:** Console-only.  
**Consequences:** Easier management; controlled exposure.

---

### ADR-0015: Multi-Factor Authentication Implementation
**Status:** Accepted  
**Decision:** Enable MFA (Microsoft Authenticator or Duo) for admins.  
**Alternatives:** Password-only.  
**Consequences:** Higher security, small usability cost.

---

## Client & Management

### ADR-0016: Workstation Operating System Selection
**Status:** Accepted  
**Decision:** Include Windows 11 and Ubuntu clients.  
**Consequences:** Broader testing and validation setup.

---

### ADR-0017: Organizational Unit and GPO Strategy
**Status:** Accepted  
**Decision:** Separate OUs for Servers, Workstations, and Users; baseline GPOs for passwords, banners, updates.  
**Consequences:** Centralized, consistent management.

---

### ADR-0018: User Account & Role Policy
**Status:** Accepted  
**Decision:** Define Admin, Standard, and Service roles with clear naming conventions.  
**Consequences:** Enforces least-privilege security.

---

### ADR-0019: Software Deployment Approach
**Status:** Accepted  
**Decision:** Use GPO or PowerShell for software rollout.  
**Consequences:** Simplified updates and provisioning.

---

### ADR-0020: Update & Patch Management
**Status:** Accepted  
**Decision:** Manual patching; evaluate WSUS or automation later.  
**Consequences:** Control retained; more manual work.

---

### ADR-0021: Endpoint Configuration Standards
**Status:** Accepted  
**Decision:** Enforce firewalls, lock screens, and non-admin user policy.  
**Consequences:** Higher security; slightly less flexibility.

---

## Security & Networking

### ADR-0022: NAT and Port Forwarding Policy
**Status:** Accepted  
**Decision:** Outbound NAT allowed; inbound restricted to DMZ.  
**Consequences:** Internet connectivity with minimal exposure.

---

### ADR-0023: Internal Firewall Ruleset Design
**Status:** Accepted  
**Decision:** Apply least-privilege rules between VLANs.  
**Consequences:** Secure, but increases troubleshooting complexity.

---

### ADR-0024: Authentication and Authorization Standards
**Status:** Accepted  
**Decision:** Use Kerberos within AD; local fallback when isolated.  
**Consequences:** Consistent domain logins across systems.

---

### ADR-0025: Logging and Auditing Framework
**Status:** Accepted  
**Decision:** Aggregate Windows Event Logs and Syslog for review.  
**Consequences:** Easier troubleshooting and auditing.

---

### ADR-0026: VPN and Secure Remote Connectivity
**Status:** Accepted  
**Decision:** Restrict remote access to internal management VLAN; VPN optional.  
**Consequences:** Reduced external attack surface.

---

## Operations, Governance & Documentation

### ADR-0027: Naming Conventions and Standards
**Status:** Accepted  
**Decision:** Use standardized host and OU naming (e.g., SRV-DC01, WK-WIN11-01).  
**Consequences:** Improved clarity and automation support.

---

### ADR-0028: Resource Allocation Strategy
**Status:** Accepted  
**Decision:** Allocate CPU/RAM based on host importance.  
**Consequences:** Optimized resource distribution.

---

### ADR-0029: Snapshot and Version Control Policy
**Status:** Accepted  
**Decision:** Maintain VirtualBox snapshots and GitHub documentation versions.  
**Consequences:** Easy rollbacks and transparent change history. More time consuming.

---

### ADR-0030: Documentation and Diagramming Methodology
**Status:** Accepted  
**Decision:** Use Markdown and Mermaid diagrams in `/docs`.  
**Consequences:** Professional, readable documentation.

---

### ADR-0031: Scalability and Future Cloud Integration
**Status:** Accepted  
**Decision:** Design for future hybrid extension (e.g., Azure AD Connect).  
**Consequences:** Minimal upfront complexity, future flexibility.

---

### ADR-0032: ADR Inclusion Policy
**Status:** Accepted  
**Context:** Define what warrants its own ADR.  
**Decision:** Record any change that affects multiple systems, topology, or security posture.  
**Alternatives Considered:** Document everything / nothing.  
**Consequences:** Keeps ADRs meaningful and maintainable.

---
