# Phase 2 - Windows Server Installation
## Proxmox VM Creation and Windows Server 2022 Setup

---

## Overview

**Goal:** Install Windows Server 2022 on a Proxmox virtual machine as preparation for Active Directory Domain Controller deployment.

**Prerequisites:**
- Proxmox VE installed and operational (version 8.x or 9.x)
- Windows Server 2022 ISO uploaded to Proxmox
- VirtIO drivers ISO available
- Minimum 60GB available storage
- Access to Proxmox web interface

**Environment Context:**
This Windows Server VM will be installed on a **Proxmox virtualization host** running on a **MINISFORUM Mini PC NAB6 Lite** with:
- Intel Core i5-12600H processor
- 16GB DDR4 RAM
- 500GB PCIe 4.0 SSD
- Dual 2.5G network interfaces

---

## Step 1: Downloaded Required ISOs

### Windows Server 2022 ISO

**Source:** Microsoft Evaluation Center  
**URL:** https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022

**Download Steps:**
1. Visited Microsoft Evaluation Center
2. Selected **Windows Server 2022 Standard**
3. **ISO - DVD** format
4. Selected language: English 
5. Downloaded evaluation version 

### VirtIO Drivers ISO

**Purpose:** Paravirtualized drivers for optimal VM performance

**Source:** Fedora Project / Proxmox  
**URL:** https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso

---

## Step 2: Upload ISOs to Proxmox

**Proxmox Web Interface:**
```
URL: https://192.168.1.100:8006
```

**Upload Process:**
1. **Datacenter / [Pve] / local (storage)**
2. **ISO Images** tab
3. Clicked **Upload** button
4. Selected Windows Server 2022 ISO 
5. Repeated for VirtIO drivers ISO

**Verification:**
- Both ISOs should appear in ISO Images list
- Status shows file size correctly

---

## Step 3: Create VM in Proxmox

### 3.1 Initial VM Creation

1. Clicked **Create VM** 
2. **General Tab:**
   - Node: [pve]
   - VM ID: `100` 
   - Name: `DC01`
   - Start at boot: checked

3. **OS Tab:**
   - Used CD/DVD disc image file (ISO): checked
   - Storage: local
   - ISO image: `SERVER_EVAL_x64FRE_en-us.iso`
   - Guest OS:
     - Type: Microsoft Windows
     - Version: 11/2022/2025

4. **System Tab:**
   - Graphics card: Default
   - Machine: q35
   - BIOS: OVMF (UEFI)
   - Add EFI Disk: checked
   - EFI Storage: local-lvm
   - SCSI Controller: VirtIO SCSI single
   - Qemu Agent: checked

5. **Disks Tab:**
   - Bus/Device: SCSI
   - Storage: local-lvm
   - Disk size (GiB): **60**
   - Cache: Write back (for better performance)
   - Discard: checked (for SSD TRIM support)
   - SSD emulation: checked (Proxmox host uses SSD)

6. **CPU Tab:**
   - Sockets: 1
   - Cores: **4**
   - Type: host (passes through host CPU features)

7. **Memory Tab:**
   - Memory (MiB): **4096** (4GB)
   - Ballooning Device: checked
   - Minimum memory: 2048 

8. **Network Tab:**
   - Bridge: vmbr0 (default bridge)
   - Model: **VirtIO (paravirtualized)**
   - VLAN Tag: blank
   - Firewall: unchecked for lab

9. **Confirm:**
   - Reviewed all settings
   - Clicked **Finish**
   - **DO NOT** start VM yet!

---

## Step 4: Add VirtIO Drivers CD

**Before starting the VM:**

1. Selected the **DC01** VM in Proxmox left panel
2. **Hardware** tab
3. Clicked **Add** / **CD/DVD Drive**
4. Configuration:
   - Bus/Device: IDE
   - Storage: local
   - ISO image: `virtio-win.iso`
5. Clicked **Add**

**Result:** VM now has two CD drives:
- Drive 0: Windows Server installation ISO
- Drive 1: VirtIO drivers ISO

---

## Step 5: Started VM and Begin Installation

### 5.1 Boot VM

1. Selected **DC01** VM
2. Clicked **Start** 
3. Clicked **Console** to open VNC console
4. VM boots from Windows Server ISO

### 5.2 Windows Server Installation Wizard

**Initial Screen:**
- Language: English (United States)
- Time and currency format: English (United States)
- Keyboard: US
- Clicked **Next**

**Install Now:**
- Clicked **Install now** button

**Product Key:**
- Clicked **"I don't have a product key"** (using evaluation copy)

**Operating System Selection:**
- Selected: **Windows Server 2022 Standard Evaluation (Desktop Experience)**
- **Desktop Experience** = GUI 
- Click **Next**

**License Terms:**
- Accepted license terms

**Installation Type:**
- Selected: **Custom: Install Windows only (advanced)**

---

## Step 6: VirtIO Storage Driver

**CRITICAL STEP:** Windows installer cannot see VirtIO disk without driver

**Disk Not Visible:**
- Installer shows: "Where do you want to install Windows?"
- No disks listed 

**Driver Installation:**
1. Clicked **Load driver** 
2. Clicked **Browse**
3. Navigated to **CD Drive (E:)** 
4. Opened: vioscsi folder
5. Opened: w2k22 folder (for Windows Server 2022)
6. Opened: amd64 folder
7. Click **OK**

**Driver Selection:**
- Driver shown: "Red Hat VirtIO SCSI controller"
- Clicked **Next**

**Result:** 60GB disk now visible in installer!

---

## Step 7: Completed Windows Installation

### 7.1 Disk Partitioning

**Disk Selection:**
- Disk 0 Unallocated Space: 60.0 GB shown
- Clicked **Next** 

**Automatic Partitions Created:**
- System partition: 100MB (EFI)
- MSR: 16MB
- Primary partition: 59.9GB (C: drive)

### 7.2 Initial Setup

**Administrator Password:**
- Created password that meet complexity requirements:
  - At least 8 characters
  - Uppercase letter
  - Lowercase letter
  - Number
  - Special character 

**Finish:**
- Clicked **Finish**
- Windows Server desktop loads

---

## Step 8: Post-Installation Configuration

### Login
- Username: Administrator
- Password: ********

### 8.2 Install VirtIO Network Driver

**Problem:** No network connectivity (VirtIO NIC driver not installed)

**Solution:**

1. Opened **File Explorer**
2. Navigated to **This PC**
3. Opened **CD Drive** with VirtIO ISO
4. Run: Guest Tools installer
5. Installation wizard opens:
   - Clicke **Next**
   - Accepted license / **Next**
   - Installed location: default / **Next**
   - Clicked **Install**
   - User Account Control: **Yes**
6. **Reboot** 

**Verification:**
- Network icon in taskbar shows connectivity
- Can reach internet

### 8.3 Configuration Of Static IP Address

1. Opened **Network and Sharing Center**
2. Clicked **Change adapter settings**
3. Right-click **Ethernet** / **Properties**
4. Selected **Internet Protocol Version 4 (TCP/IPv4)**
5. Clicked **Properties**
4. **Used the following IP address:**
   - IP address: 192.168.1.10
   - Subnet mask: 255.255.255.0
   - Default gateway: 192.168.1.1
7. **Used the following DNS server addresses:**
   - Preferred: 127.0.0.1 (loopback address,for future DC)
   - Alternate: 1.1.1.1 (temporary, for installation)
8. Clicked **OK** and **Close**

**Tested Connectivity:**
```powershell
# Test gateway
Test-NetConnection -ComputerName 192.168.1.1

# Test internet
Test-NetConnection -ComputerName google.com
```

### 8.4 Rename Server

**Renamed to DC01:**

**GUI Method:**
1. Right-click **Start** and **System**
2. Clicked **Rename this PC**
3. Entered: DC01
4. Clicked **Next**
5. Clicked **Restart now**

**Result:** Server reboots with hostname DC01

---

## Step 9: Installation of QEMU Guest Agent

**Purpose:** Enables better Proxmox-VM integration 

**Installation:**

1. Opened VirtIO drivers CD in Explorer
2. Navigate to: guest-agent folder
3. Run: Quemu installer
4. Installation wizard:
   - Click **Next**
   - Accept license / **Next**
   - **Install**
   - **Finish**

**Verification in Proxmox:**
1. Selected DC01 VM
2. **Summary** tab
3. **IPs** field - 192.168.1.10
4. **Guest Agent** status: Enabled and running 

---

## Step 10: Windows Updates

**Windows Update:**

1. Opened **Settings** / **Windows Update**
2. Clicked **Check for updates**
3. Installed all available updates

**Critical Updates:**
- .NET Framework updates
- Security updates
- Quality updates
- Feature updates 
---

## Verification Checklist

**VM Configuration:**
- [x] VM ID: 100
- [x] VM Name: DC01
- [x] vCPU: 4 cores
- [x] RAM: 4GB
- [x] Disk: 60GB (SCSI, VirtIO)
- [x] Network: VirtIO model
- [x] QEMU Guest Agent: Installed and running

**Windows Server:**
- [x] OS: Windows Server 2022 Standard (Desktop Experience)
- [x] Hostname: DC01
- [x] Administrator password set and saved
- [x] Static IP: 192.168.1.10
- [x] Gateway: 192.168.1.1
- [x] DNS: 127.0.0.1 (primary), 1.1.1.1 (secondary)
- [x] Network connectivity working
- [x] Windows updates installed

**Drivers:**
- [x] VirtIO SCSI driver (storage)
- [x] VirtIO Network driver (NIC)
- [x] QEMU Guest Agent installed

---

## Issues and Troubleshooting

### Issue 1: Disk Not Visible During Installation

**Cause:** VirtIO SCSI driver not loaded

**Solution:** Follow Step 6 carefully - load driver from vioscsi\w2k22\amd64\

### Issue 2: No Network Connectivity After Install

**Cause:** VirtIO network driver not installed

**Solution:** Install virtio-win-gt-x64.msi from VirtIO ISO

### Issue 3: QEMU Guest Agent Not Working

**Symptoms:** Proxmox can't see VM IP address

**Solution:**
1. Verify Guest Agent is installed in VM
2. Check service is running: Get-Service "QEMU-GA"
3. Restart service if needed: Restart-Service "QEMU-GA"
4. Verify in Proxmox: Hardware / Options / QEMU Guest Agent: Enabled


**Phase 2 Status:** ✅ Reference Documentation  
**Next Phase:** Phase 3 - Domain Controller Promotion  
**Created:** December 2, 2025  
**Lab Environment:** Proxmox on MINISFORUM Mini PC NAB6 Lite

**Status:** ✅ COMPLETE - Windows Server 2022 installed on DC01
**Next Phase:** Phase 3 - Domain Controller Configuration
