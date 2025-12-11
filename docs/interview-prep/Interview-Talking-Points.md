# Active Directory Lab - Interview Preparation Guide

**Purpose:** Interview questions and answers based on actual lab experience

## Overview

This guide contains real interview questions you can confidently answer using your Active Directory lab experience, organized by topic and using the STAR method for behavioral questions.

## Quick Reference

### Top Technical Questions You Can Answer

1. **"Tell me about your Active Directory experience"**
   - Built complete AD lab from scratch
   - Windows Server 2022 domain controller
   - lab.local domain with full DNS
   - Domain-joined Windows 10 clients
   - OU structure, users, groups, GPOs

2. **"How do you create a user account in AD?"**
   - Both GUI (ADUC) and PowerShell methods
   - Created test users in Lab Users OU
   - Understand password policies and complexity
   - Know about UPN vs SAM account names

3. **"How do you troubleshoot domain join issues?"**
   - Check DNS configuration first (most common cause)
   - Verify network connectivity to DC
   - Test DNS resolution with nslookup
   - Check firewall rules
   - Successfully resolved this in your lab

4. **"What is Group Policy and how do you use it?"**
   - Implemented 5 GPOs in lab
   - Password policy, desktop wallpaper, drive mapping
   - Understand Computer vs User Configuration
   - Know troubleshooting with gpresult and gpupdate

5. **"Explain the difference between OUs and security groups"**
   - OUs = organizational structure and policy application
   - Groups = access control and permissions
   - Created both in lab environment

## Key Lab Achievements for Interviews

### What You've Built
- ✅ Windows Server 2022 Domain Controller (DC01)
- ✅ lab.local domain with DNS
- ✅ Organizational Unit structure
- ✅ Multiple test user accounts
- ✅ Security groups
- ✅ Domain-joined Windows 10 client
- ✅ Group Policy Objects (5 different types)
- ✅ File server with NTFS permissions

### Skills You Can Demonstrate
- Windows Server administration
- Active Directory management
- DNS configuration and troubleshooting
- User and group management
- Group Policy implementation
- Network configuration
- Systematic troubleshooting methodology
- Technical documentation

## Best Troubleshooting Stories

### Story 1: Domain Join DNS Issue (STAR Format)

**Situation:** Windows 10 client couldn't join lab.local domain

**Task:** Diagnose why domain controller wasn't accessible

**Action:**
- Verified network connectivity with ping
- Checked DNS configuration - found it pointing to 8.8.8.8 instead of DC
- Reconfigured to use 192.168.1.10 (DC) as primary DNS
- Tested with nslookup lab.local
- Successfully joined domain

**Result:** Learned DNS is #1 cause of domain join failures

### Story 2: GPO Not Applying (STAR Format)

**Situation:** Screen lock policy wasn't applying to domain computers

**Task:** Troubleshoot why Group Policy wasn't taking effect

**Action:**
- Ran gpresult /r to check applied policies
- Discovered policy was linked to Computers OU
- Learned User Configuration policies must link to Users OU
- Moved GPO link from Lab Computers to Lab Users OU
- Ran gpupdate /force and verified with gpresult

**Result:** Understood critical difference between Computer and User Configuration policies

### Story 3: Remote Desktop Permission Issue (STAR Format)

**Situation:** Domain user could log in at console but not via RDP

**Task:** Determine why RDP failed for valid domain account

**Action:**
- Verified account was active in AD
- Checked firewall rules
- Discovered domain users don't automatically get RDP rights
- Added user to local Remote Desktop Users group
- Successfully connected via RDP

**Result:** Learned that domain membership doesn't automatically grant all access rights

## Technical Questions You're Prepared For

### Active Directory Basics
- What is Active Directory?
- What is a domain controller?
- Explain OUs vs security groups
- What are FSMO roles?
- How does DNS integrate with AD?

### User Management
- How to create user accounts
- Password reset procedures
- Account lockout troubleshooting
- Group membership management
- Understanding user properties (UPN, SAM)

### Group Policy
- What is Group Policy?
- Computer vs User Configuration
- How to create and link GPOs
- Troubleshooting with gpresult
- Common GPO examples (drive mapping, security policies)

### Troubleshooting
- Domain join failures
- DNS issues
- Authentication problems
- Network connectivity
- Policy application issues

## Behavioral Questions Using Lab Examples

### "Tell me about a time you learned something new"
**Use:** Your entire AD lab project as an example of self-directed learning

### "Describe a time you had to be persistent"
**Use:** Password/authentication troubleshooting story

### "Give an example of troubleshooting a complex issue"
**Use:** RDP permission issue or DNS domain join problem

### "When did you have to learn from a mistake?"
**Use:** Any troubleshooting scenario where first attempt failed

## Quick Answers for Common Questions

**"Why should we hire you without IT experience?"**
- 10 years customer service from healthcare
- Professional maturity and reliability
- Active learning (CCNA in progress, AD lab built)
- Systematic troubleshooting skills
- Genuine enthusiasm for IT career

**"What's your greatest technical accomplishment?"**
- Built enterprise-grade AD lab environment
- Solved multiple real troubleshooting scenarios
- Documented everything professionally
- Demonstrates initiative and persistence

**"Why are you transitioning to IT?"**
- Always interested in technology
- Want continuous learning opportunities
- Enjoy systematic problem-solving
- Healthcare taught customer service foundation
- IT offers long-term career growth path

## Portfolio Evidence You Can Show

### Available Screenshots
- Domain-joined workstation
- Group membership verification
- Network configuration
- AD computer objects in OUs
- Successful RDP connections

### Documentation Available
- Phase-by-phase lab build notes
- Troubleshooting scenarios with solutions
- PowerShell commands reference
- This GitHub repository!

## Questions to Ask Them

1. "What Active Directory version does your environment use?"
2. "How is your OU structure organized?"
3. "What are the typical day-to-day AD tasks for Level 1?"
4. "Do you use Group Policy extensively?"
5. "What opportunities are there to learn advanced AD concepts?"
6. "How does your team handle password resets - through AD or ticketing system?"

## Interview Day Checklist

**Before Interview:**
- [ ] Review job description for AD-related keywords
- [ ] Practice 2-3 STAR stories out loud
- [ ] Have GitHub repo link ready to share
- [ ] Prepare specific questions about their AD environment
- [ ] Review technical terms from job posting

**During Interview:**
- [ ] Reference specific lab examples
- [ ] Use proper technical terminology
- [ ] Be clear about lab vs production experience
- [ ] Show enthusiasm for continued learning
- [ ] Ask technical questions about their environment

**Remember:**
- Lab experience IS valuable - it's hands-on work
- Troubleshooting stories show your problem-solving process
- Your healthcare customer service background is an asset
- Professional maturity matters
- Enthusiasm + willingness to learn = great candidate

---

**For complete interview stories and detailed STAR examples, see the full documentation in Obsidian vault or project files.**

*This document complements your resume and provides concrete talking points for technical interviews.*
