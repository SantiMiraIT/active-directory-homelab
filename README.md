# Active Directory Home Lab Project

## 🎯 Project Overview

This repository documents my hands-on Active Directory home lab project, built as part of my transition from healthcare to IT support roles. The lab demonstrates practical enterprise IT skills including Windows Server administration, Active Directory management, Group Policy configuration, and PowerShell automation.

## 👨‍💼 About Me

I'm Santino, a professional transitioning from a 10-year healthcare career (remedial massage therapist and business owner) to IT support and networking. I'm currently:
- 📚 Studying for CCNA certification (30% complete, expected Q2 2026)
- 🖥️ Building hands-on lab environments to develop practical skills
- 🎯 Targeting Help Desk Level 1 positions in Sydney, Australia
- 🔧 Leveraging strong customer service and systematic problem-solving skills from healthcare

## 🏗️ Lab Environment

**Hardware:**
- **Hypervisor**: Proxmox VE 9 on MINISFORUM Mini PC
  - Intel Core i5-12600H (12 Cores, 16 Threads, up to 4.5GHz)
  - 16GB DDR4 RAM
  - 500GB PCIe 4.0 SSD
- **Networking**: TP-Link Archer VR1600v Router, TP-Link Omada ES205GP Managed PoE Switch

**Virtual Machines:**
- **DC01**: Windows Server 2022 (Domain Controller)
  - IP: 192.168.1.10
  - Domain: lab.local
  - Role: Active Directory Domain Services, DNS, DHCP
- **WIN10-CLI**: Windows 10 Pro (Client Workstation)
  - IP: 192.168.1.20
  - Domain-joined to lab.local

## 📚 Lab Phases Completed

### Phase 1: Lab Planning and Design
- Network topology design
- IP addressing scheme
- Domain naming conventions
- Hardware and software requirements

### Phase 2: Windows Server Installation
- Proxmox VM configuration
- Windows Server 2022 installation
- VirtIO driver implementation
- Static IP configuration

### Phase 3: Domain Controller Configuration
- Active Directory Domain Services installation
- DNS configuration and validation
- Forest and domain creation (lab.local)
- Organizational Unit (OU) structure design

### Phase 4: Client Workstation Setup
- Windows 10 Pro installation and configuration
- Domain join procedures
- User profile configuration
- Network connectivity validation

### Phase 5: File Server and Permissions
- Shared folder creation and configuration
- NTFS permissions management
- Share permissions implementation
- Access control testing

### Phase 6: Group Policy Objects (GPO)
- Password policy configuration
- Desktop customization via GPO
- Drive mapping automation
- Software restriction policies
- Screen lock policies
- GPO troubleshooting and validation

## 💡 Key Skills Demonstrated

**Windows Server Administration:**
- Active Directory Domain Services configuration
- DNS and DHCP management
- User and computer account management
- Organizational Unit (OU) design and implementation

**Group Policy Management:**
- Password and account policies
- Drive mapping automation
- Security settings configuration
- GPO troubleshooting with `gpresult` and `gpupdate`

**Networking Fundamentals:**
- Static IP configuration
- DNS resolution and troubleshooting
- Domain name services
- Network connectivity diagnosis

**Problem-Solving & Troubleshooting:**
- Systematic troubleshooting methodology
- Log analysis and diagnostic tools
- Policy application debugging
- Documentation of solutions

**PowerShell Basics:**
- Active Directory cmdlets
- User account management scripts
- Automated reporting

## 📖 Documentation Structure

```
├── docs/
│   ├── fundamentals/          # Core Active Directory concepts
│   ├── phases/                # Step-by-step lab implementation guides
│   ├── powershell/            # PowerShell automation scripts and guides
│   └── interview-prep/        # Interview talking points and technical questions
└── scripts/                   # PowerShell scripts (future)
```

## 🎓 Learning Outcomes

Through this lab project, I've gained:

1. **Practical Experience**: Hands-on implementation of enterprise Active Directory environments
2. **Troubleshooting Skills**: Real-world problem diagnosis and resolution
3. **Documentation Skills**: Professional technical documentation for knowledge transfer
4. **Systems Thinking**: Understanding how AD components interact in enterprise environments
5. **Professional Maturity**: Systematic approach to complex technical projects

## 🎯 Career Goals

**Short-term (3-6 months):**
- Secure Help Desk Level 1 position in Sydney
- Complete CCNA certification
- Continue expanding lab environment with advanced scenarios

**Long-term (2-5 years):**
- Progress to Network Administrator/Engineer roles
- Achieve CCNP certification
- Specialize in enterprise networking and infrastructure

## 📞 Contact

**LinkedIn**: [Connect with me on LinkedIn](#) <!-- Add your LinkedIn URL -->
**Location**: Sydney, Australia
**Email**: [Your professional email] <!-- Add your email -->

---

## 🙏 Acknowledgments

This lab project was built through self-directed learning, leveraging resources including:
- Microsoft documentation
- CCNA study materials
- IT community forums and best practices
- Real-world troubleshooting scenarios

---

**Note to Recruiters**: This repository demonstrates my commitment to continuous learning and practical skill development. Each phase includes detailed documentation showing not just what I built, but how I troubleshot issues and validated solutions - skills directly applicable to help desk support roles.

*Last Updated: December 2025*