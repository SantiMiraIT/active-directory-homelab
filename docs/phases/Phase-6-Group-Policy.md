---
tags:
  - active-directory
  - group-policy
  - gpo
  - phase-6
created: 2025-12-10
status: complete
phase: "6"
---

# Phase 6 - Group Policy Objects
## GPO Configuration and Management

**Status:** ✅ COMPLETE

## Summary

Implemented five core Group Policy Objects demonstrating enterprise IT skills:

### 1. Password Policy GPO
- Minimum password length: 8 characters
- Password complexity requirements enforced
- Linked to: Lab Users OU

### 2. Desktop Wallpaper GPO
- Custom wallpaper deployment
- Prevents user modification
- Demonstrates desktop standardization

### 3. Drive Mapping GPO (H: Drive)
- Automated network drive mapping
- User-specific home folders
- **Most common help desk GPO task**
- Action: Replace (for reliable application)

### 4. Software Restriction Policy
- Blocks execution from TEMP folders
- Security hardening demonstration
- Path-based restrictions

### 5. Screen Lock Policy
- Auto-lock after 5 minutes inactivity
- Linked to: Lab Users OU (User Configuration)

## Key Learning

**Critical Discovery:**
- User Configuration policies must be linked to Users OU
- Computer Configuration policies link to Computers OU
- Troubleshot with `gpresult /r` and `gpupdate /force`

## Skills Demonstrated

- Group Policy Object creation and configuration
- Policy linking to Organizational Units
- GPO troubleshooting with gpresult
- Understanding Computer vs User Configuration
- Drive mapping automation (critical help desk skill)
- Security policy implementation

**Next Steps:** PowerShell Automation (Phase 8A)
