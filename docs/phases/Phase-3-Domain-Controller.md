
# Phase 2 - Windows Server Installation
## Proxmox VM Creation and Windows Server 2022 Setup

---

## Overview

**Goal:** Install Windows Server 2022 on a Proxmox virtual machine as preparation for Active Directory Domain Controller deployment.

**Prerequisites:**
- Proxmox VE installed and operational 
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

## Step 1: Download Required ISOs

**Download Steps:**
1. Visited Microsoft Evaluation Center
2. Selected **Windows Server 2022 Standard**
3. Chose **ISO - DVD** format
4. Selected language: English 
5. Downloaded evaluation version (180-day trial)

### VirtIO Drivers ISO

**Purpose:** Paravirtualized drivers for optimal VM performance

**Source:** Fedora Project / Proxmox  

**File Details:**
- Includes: Storage, network, and display drivers

---

## Step 2: Uploaded ISOs to Proxmox

**Access Proxmox Web Interface:**
```
URL: https://192.168.1.100:8006
Username: root
Password: ******
```

**Upload Process:**
1. Navigated to: **Datacenter / [Pve] / local (storage)**
2. Selected **ISO Images** tab
3. Clicked **Upload** button
4. Selected Windows Server 2022 ISO / **Upload**
5. Repeat for VirtIO drivers ISO

**Verification:**
- Both ISOs appeared in ISO Images list
- Status shows file size correctly

---

## Step 3: VM Creation in Proxmox

### 3.1 Initial VM Creation

1. Clicked **Create VM** 
2. **General Tab:**
   - Node: Pve
   - VM ID: `100` (or next available)
   - Name: `DC01`
   - Start at boot: checked

3. **OS Tab:**
   - Used CD/DVD disc image file (ISO)
   - Storage: local
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
   - SSD emulation: checked

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
   - Firewall: unchecked (unchecked for lab)

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
4. Configure:
   - Bus/Device: IDE
   - Storage: local
   - ISO image: `virtIO iso image`
5. Clicked **Add**

**Result:** VM now has two CD drives:
- Drive 0: Windows Server installation ISO
- Drive 1: VirtIO drivers ISO

---

## Step 5: Started VM and Begin Installation

### 5.1 Boot VM

1. Selected **DC01** VM
2. Clicked **Start** (top-right)
3. Clicked **Console** to open VNC console
4. VM booted from Windows Server ISO

### 5.2 Windows Server Installation Wizard

**Initial Screen:**
- Language: English 
- Time and currency format: English 
- Keyboard: US
- Clicked **Next**

**Install Now:**
- Clicked **Install now** button

**Product Key:**
- Clicked **"I don't have a product key"** (using evaluation)

**Operating System Selection:**
- Selected: **Windows Server 2022 Standard Evaluation (Desktop Experience)**
- **Desktop Experience** = GUI (not Core)
- Clicked **Next**

**License Terms:**
- Accepted license terms
- Clicked **Next**

**Installation Type:**
- Selected: **Custom: Install Windows only (advanced)**

---

## Step 6: Loaded VirtIO Storage Driver

**CRITICAL STEP:** Windows installer cannot see VirtIO disk without driver

**Disk Not Visible:**
- Installer shows: "Where do you want to install Windows?"
- No disks listed 

**Loaded Driver:**
1. Clicked **Load driver** (bottom-left)
2. Clicked **Browse**
3. Navigate to **CD Drive (E:)** or similar (VirtIO ISO)
4. Opened: `vioscsi` folder
5. Opened: `w2k22` folder 
6. Opened: `amd64` folder
7. Click **OK**

**Select Driver:**
- Driver shown: "Red Hat VirtIO SCSI controller"
- Clicked **Next**

**Result:** 60GB disk now visible in installer

---

## Step 7: Completed Windows Installation

### 7.1 Disk Partitioning

**Disk Selection:**
- Disk 0 Unallocated Space: 60.0 GB shown
- Clicked **Next** (Windows creates partitions automatically)

**Automatic Partitions Created:**
- System partition: 100MB (EFI)
- MSR (Microsoft Reserved): 16MB
- Primary partition: 59.9GB (C: drive)

### 7.2 Installation Progress

**Copying Windows files:** 

**Automatic Reboot:**
- VM will rebooted automatically

### 7.3 Initial Setup

**Administrator Password:**
- Must meet complexity requirements:
  - At least 8 characters
  - Uppercase letter
  - Lowercase letter
  - Number
  - Special character (optional but recommended)

**Finish:**
- Clicked **Finish**
- Windows Server desktop loads

---

## Step 8: Post-Installation Configuration

### 8.1 Pressed Ctrl+Alt+Delete

**In Proxmox Console:**
-  Selected **"Ctrl-Alt-Delete"** from Console dropdown

**Login:**
- Username: `Administrator`
- Password: ******
- Pressed **Enter**

### 8.2 Install VirtIO Network Driver

**Problem:** No network connectivity (VirtIO NIC driver not installed)

**Solution:**

1. Opened **File Explorer**
2. Navigateed to **This PC**
3. Opened **CD Drive** with VirtIO ISO
4. Run: Guest Tools installer)
5. Installation wizard opens:
   - Clicked **Next**
   - Accepted license / **Next**
   - Install location: default / **Next**
   - Clicked **Install**
   - User Account Control: **Yes**
6. **Finish** and **Reboot** 

**Verification:**
- Network icon in taskbar shows connectivity
- Can reach internet

### 8.3 Configure Static IP Address

```
**Method: GUI**

1. Opened **Network and Sharing Center**
2. Clicked **Change adapter settings**
3. Right-click **Ethernet** → **Properties**
4. Selected **Internet Protocol Version 4 (TCP/IPv4)**
5. Clicked **Properties**
6. Selected **Use the following IP address:**
   - IP address: `192.168.1.10`
   - Subnet mask: `255.255.255.0`
   - Default gateway: `192.168.1.1`
7. Select **Use the following DNS server addresses:**
   - Preferred: `127.0.0.1` (loop-back, for future DC)
 8. Click **OK** → **Close**

**Test Connectivity:**
```powershell
# Test gateway
Test-NetConnection -ComputerName 192.168.1.1

# Test internet
Test-NetConnection -ComputerName google.com
```

### 8.4 Renaming Server

**Renamed to DC01:**

```
**GUI Method:**
1. Right-click **Start** → **System**
2. Clicked **Rename this PC**
3. Enter: `DC01`
4. Clicked **Next**
5. Clicked **Restart now**

**Result:** Server rebooted with hostname DC01

---

## Step 9: Installed QEMU Guest Agent

**Purpose:** Enables better Proxmox-VM integration (shutdown, IP detection, etc.)

**Installation:**

1. Opened VirtIO drivers CD in Explorer
2. Navigateed to: `guest-agent` folder
3. Run: `qemu-ga-x86_64.msi`
4. Installation wizard:
   - Clicked **Next**
   - Accepted license → **Next**
   - **Install**
   - **Finish**

**Verification in Proxmox:**
1. Select DC01 VM
2. Go to **Summary** tab
3. Look for **IPs** field - shows 192.168.1.10
4. **Guest Agent** status: Enabled and running 

---

## Step 10: Windows Updates

**Run Windows Update:**

1. Opened **Settings** → **Windows Update**
2. Clicked **Check for updates**
3. Installed all available updates
4. Rebooted

**Critical Updates:**
- .NET Framework updates
- Security updates
- Quality updates

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

## Common Issues and Troubleshooting

### Issue 1: Disk Not Visible During Installation

**Cause:** VirtIO SCSI driver not loaded

**Solution:** loaded driver

### Issue 2: No Network Connectivity After Install

**Cause:** VirtIO network driver not installed

**Solution:** Installed virtIO drivers from VirtIO ISO

### Issue 3: QEMU Guest Agent Not Working

**Symptoms:** Proxmox can't see VM IP address

**Solution:**
1. Verifed Guest Agent is installed in VM
2. Checked service is running: `Get-Service "QEMU-GA"`
3. Restarted service if needed: `Restart-Service "QEMU-GA"`
4. Verified in Proxmox: Hardware / Options / QEMU Guest Agent: Enabled

---

