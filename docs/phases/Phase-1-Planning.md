---
tags:
  - active-directory
  - home-lab
  - planning
  - phase-1
  - lab-design
created: 2025-12-02
modified: 2025-12-02
status: reference
phase: "1"
project: Active Directory Lab
difficulty: Beginner
duration: 1-2 hours
---

# Phase 1 - Active Directory Lab Planning and Design
## Project Overview and Infrastructure Planning

### Project Purpose
**Career Goal Alignment**:
This Active Directory lab project directly supports my transition to IT help desk and eventual networking career by providing:

**Hands-on Active Directory experience** - Critical for 85% of help desk positions
**Windows Server administration skills** - Required knowledge for IT support roles
**Networking fundamentals practice** - Foundation for CCNA and networking career
**Portfolio-ready documentation** - Concrete examples for resume and interviews

---

## Lab Environment Overview

### Hardware Infrastructure

**Primary Server:**
- **Device:** MINISFORUM Mini PC NAB6 Lite
- **Processor:** Intel Core i5-12600H (12 Cores, 16 Threads, up to 4.5GHz)
- **RAM:** 16GB DDR4
- **Storage:** 500GB PCIe 4.0 SSD
- **Networking:** Dual 2.5G RJ45 LAN ports
- **Additional:** 2x HDMI, 7x USB ports, WiFi 6, Bluetooth 5.2

**Virtualization Platform:**
- **Software:** Proxmox VE (currently version 9.1.1)
- **Purpose:** Hosts all virtual machines for the lab
- **Location:** Installed on MINISFORUM Mini PC

**Network Equipment:**
- **Router:** TP-Link Archer VR1600v
- **Managed Switch:** TP-Link Omada 5-Port Gigabit (ES205GP)
  - 4x PoE+ ports
  - 65W power budget
  - VLAN support
  - Cloud management capable
- **Primary Workstation:** MacBook M1 (8GB RAM, 245GB storage)
  - Static IP: 192.168.1.50
- **Monitor:** Dell P2721Q

---

## Network Design

### IP Addressing Scheme

**Network:** 192.168.1.0/24

**Static IP Assignments:**
- `192.168.1.10` - DC01 (Domain Controller)
- `192.168.1.20` - WIN10-CLI (Windows 10 Client)
- `192.168.1.50` - MacBook (Primary workstation)
- `192.168.1.100` - Proxmox Host
- `192.168.1.1` - Router/Gateway

**DNS Configuration:**
- **Primary DNS:** 192.168.1.10 (DC01 - internal)
- **Secondary DNS:** Blank (self-referential DC)
- **External DNS Forwarders:** Configured on DC01 to forward to ISP/public DNS

---

## Virtual Machine Architecture

### Domain Controller (DC01)

**Purpose:** Active Directory Domain Services, DNS, File Services

**Specifications:**
- **OS:** Windows Server 2022 Standard (Desktop Experience)
- **vCPU:** 4 cores
- **RAM:** 4GB
- **Disk:** 60GB (SCSI, VirtIO driver)
- **Network:** VirtIO (paravirtualized)
- **IP:** 192.168.1.10 (static)
- **Hostname:** DC01
- **Domain:** lab.local
- **Role:** Domain Controller, DNS Server, File Server

**Key Configurations:**
- Domain: lab.local
- NetBIOS: LAB
- Forest/Domain Functional Level: Windows Server 2022
- QEMU Guest Agent: Enabled
- VirtIO drivers: Installed

### Windows 10 Client (WIN10-CLI)

**Purpose:** Domain-joined workstation for testing user access and policies

**Specifications:**
- **OS:** Windows 10 Pro (or Windows 11 Pro)
- **vCPU:** 2-4 cores
- **RAM:** 4GB
- **Disk:** 40-50GB
- **Network:** VirtIO
- **IP:** 192.168.1.20 (static)
- **Hostname:** WIN10-CLI
- **Domain Membership:** lab.local

**Purpose:**
- Test domain authentication
- Apply Group Policies
- Simulate end-user scenarios
- Practice help desk troubleshooting

---

## Active Directory Structure Design

### Domain Configuration

**Domain Name:** lab.local  
**NetBIOS Name:** LAB  
**DNS Zones:**
- Forward Lookup: lab.local
- Reverse Lookup: 1.168.192.in-addr.arpa

### Organizational Unit (OU) Structure

```
lab.local
├── Domain Controllers (default)
├── Lab Users
│   ├── John Doe (jdoe) - Test user
│   ├── Frank Finance (ffinance)
│   ├── Helen HR (hhr)
│   └── Mike Manager (mmanager)
├── Lab Computers
│   └── WIN10-CLI
├── Lab Servers
│   └── (Future servers)
└── Lab Groups
    ├── IT Support
    ├── Finance
    ├── HR
    └── Managers
```

**OU Design Rationale:**
- **Lab Users:** All user accounts for organized management
- **Lab Computers:** Domain-joined workstations
- **Lab Servers:** Member servers (future expansion)
- **Lab Groups:** Security and distribution groups

**Best Practice:** All OUs protected from accidental deletion

### Security Groups

**IT Support:**
- **Type:** Global Security
- **Purpose:** Administrative access to IT resources
- **Members:** John Doe (jdoe)

**Finance:**
- **Type:** Global Security
- **Purpose:** Access to Finance shared folders
- **Members:** Frank Finance (ffinance)

**HR:**
- **Type:** Global Security
- **Purpose:** Access to HR shared folders
- **Members:** Helen HR (hhr)

**Managers:**
- **Type:** Global Security
- **Purpose:** Access to Leadership shared folders
- **Members:** Mike Manager (mmanager)

---

## Project Phases Overview

### Phase 1: Planning and Design (This Document) ✅
- Define project scope and goals
- Design network architecture
- Plan VM specifications
- Design OU structure

### Phase 2: Windows Server Installation (on Proxmox VM) ✅
- Create DC01 VM in Proxmox
- Install Windows Server 2022
- Configure static IP and networking
- Install VirtIO drivers and Guest Agent
- Rename server to DC01

### Phase 3: Domain Controller Promotion ✅
- Promote server to Domain Controller
- Configure lab.local domain
- Set up DNS services
- Create initial OU structure
- Create test user accounts

### Phase 4: Client Workstation Setup ✅
- Deploy Windows 10/11 VM
- Configure static IP
- Join WIN10-CLI to lab.local domain
- Test domain authentication
- Verify DNS resolution

### Phase 5: File Server Configuration ✅
- Install File Server role on DC01
- Create departmental shared folders
- Configure NTFS permissions
- Implement group-based access control
- Test security from client

### Phase 6: Group Policy Introduction ✅
- Create first Group Policy Objects
- Link GPOs to OUs
- Configure basic policies
- Test policy application
- Verify with gpresult

### Phase 7: Advanced Features (Future) ✅
- Group Policy advanced settings
- PowerShell automation
- Backup and recovery
- Hybrid Azure AD Connect
- Additional services

---

## Success Criteria

**Technical Milestones:**
- [X] Proxmox virtualization platform operational
- [X] DC01 domain controller functional
- [X] lab.local domain accessible
- [X] DNS resolving internal and external queries
- [X] Client computer joined to domain
- [X] Users can authenticate from client
- [X] File shares accessible with proper permissions
- [X] Group Policies applying correctly

**Career Development Milestones:**
- [X] Comprehensive documentation completed
- [X] Screenshots captured for portfolio
- [X] Interview talking points prepared
- [X] STAR stories written for experiences
- [X] Resume bullets updated with lab projects
- [X] LinkedIn profile updated with skills

---

## Resume Talking Points

**Project Summary Statement:**
*"Built enterprise-grade Active Directory lab environment using Windows Server 2022 on Proxmox virtualization platform. Configured domain controller, implemented organizational structure, created user accounts and security groups, deployed file server with granular NTFS permissions, and applied Group Policy Objects. Documented entire process for portfolio and career development."*

**Key Skills Demonstrated:**
- Windows Server 2022 administration
- Active Directory Domain Services
- DNS configuration and troubleshooting
- User and group management
- File server permissions (NTFS and Share)
- Group Policy management
- Virtualization (Proxmox)
- Network planning and implementation
- PowerShell for verification and automation
- Documentation and technical writing

---

## Resources and References

**Official Documentation:**
- Microsoft Learn: Windows Server
- Microsoft Docs: Active Directory Best Practices
- Proxmox VE Documentation

**Learning Resources:**
- CCNA study materials (networking foundation)
- IT Career Transition Guide (career strategy)
- Lab project documentation (this folder)

---

## Project Timeline

**Estimated Duration:** 2-4 weeks (working part-time)

**Week 1:**
- Complete planning and design
- Install and configure Windows Server VM
- Promote to Domain Controller

**Week 2:**
- Deploy Windows 10 client
- Join to domain
- Create users and groups

**Week 3:**
- Configure file server
- Set up permissions
- Test access controls

**Week 4:**
- Implement Group Policies
- Complete documentation
- Prepare portfolio materials

---

## Notes and Considerations

**Lab Environment Context:**
- This is a home lab for learning, not production
- Security settings optimized for learning (e.g., "Password never expires")
- All services running on single domain controller (not recommended for production)
- Proxmox hosted on dedicated MINISFORUM server hardware

**Why NOT AWS/Azure Cloud:**
- Physical hardware provides better learning experience
- No monthly cloud costs
- Can practice disaster recovery and backup procedures
- More realistic for small business environments
- Better for CCNA networking practice

**Expansion Possibilities:**
- Additional domain controllers (redundancy)
- Member servers for specific roles
- VLAN segmentation with managed switch
- pfSense firewall VM
- Hybrid Azure AD Connect
- Additional client operating systems

---



**Status:** ✅ COMPLETE - Planning and Design completed
**Next Phase:** Phase 2 - Windows Server Installation
