# Windows 10 to Windows 11 Pro Upgrade
## Change Management, Implementation & Lessons Learned

**Project Date:** November 27, 2025  
**Learning Path:** CompTIA Security+ (Self-Directed)  
**Background:** ILS Student — Cybersecurity Career Transition  
**Repository:** [uriel0byte](https://github.com/uriel0byte/)

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Change Management Plan](#change-management-plan)
3. [Implementation & Procedures](#implementation--procedures)
4. [Challenges Encountered & Solutions](#challenges-encountered--solutions)
5. [Post-Implementation Review & Lessons Learned](#post-implementation-review--lessons-learned)
6. [Security+ Concepts Applied](#security-concepts-applied)
7. [Performance Metrics & Verification](#performance-metrics--verification)
8. [References & Resources](#references--resources)

---

## Executive Summary

This document covers a Windows 10 Home to Windows 11 Pro in-place upgrade performed on a personal cybersecurity workstation. The primary drivers were Windows 10 reaching end-of-life on October 14, 2025, and the need for BitLocker and EFS — features unavailable on the Home edition. The upgrade ran overnight on November 27, completed in 3 hours 17 minutes across three automatic restarts, and all files were preserved. Post-upgrade work included full BitLocker encryption, UAC hardening to maximum, telemetry service disabling, account privilege separation, DNS configuration, and bloatware removal. All issues encountered were diagnosed and resolved independently.

**Timeline:** November 27–28, 2025 (3.5 hours total)

---

## Change Management Plan

### 1. Change Description

| Field | Detail |
|-------|--------|
| Current State | Windows 10 Home |
| Target State | Windows 11 Pro with security hardening |
| Scope | Single personal workstation (cybersecurity learning environment) |
| Implementation Date | November 27, 2025 |

### 2. Business Justification

- Windows 10 support ended October 14, 2025; remaining on an unsupported OS means unpatched CVEs with no vendor remediation path.
- BitLocker and EFS are required for encrypting sensitive project files and are only available on Pro and above.
- The migration itself serves as a hands-on change management exercise aligned with Security+ Domain 4 and 5 objectives.

### 3. Impact Analysis

**Technical Impact**

- Estimated downtime: 3–4 hours (scheduled overnight)
- Data scope: ~170 GB across personal files and cybersecurity project directories
- Services affected: local dev environment, VirtualBox hypervisor, learning materials
- Rollback window: 10 days (Windows built-in revert)

**Security Impact**

- Gains: BitLocker full-disk encryption, TPM 2.0 integration, stronger UAC policy, telemetry controls
- Temporary risk: potential driver instability during transition (anticipated, managed)

### 4. Risk Assessment

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|-----------|
| File loss during upgrade | Low | Critical | Full backup to external drive + cloud sync |
| BitLocker lockout (recovery key lost) | Very Low | Critical | Recovery key stored in 3 separate locations |
| Driver incompatibilities | Medium | High | OEM drivers pre-downloaded before upgrade |
| Application failures | Medium | Medium | Compatibility verified; tested post-upgrade |
| Performance degradation | Low | Medium | Disk cleanup pre-upgrade; optimization post-upgrade |

### 5. Pre-Upgrade Checklist

- ✅ TPM 2.0 confirmed enabled (`tpm.msc`)
- ✅ BitLocker recovery key location verified (`account.microsoft.com/devices/recoverykey`)
- ✅ Full system backup completed — 156 GB to external Seagate 1TB, spot-checked
- ✅ Cloud backup — 12.5 GB of cybersecurity projects synced to Google Drive
- ✅ OEM drivers pre-downloaded: Nvidia 546.29, Realtek RTL8111H, Intel Chipset, Realtek HD Audio
- ✅ Windows language/region set to English (United States) — required for upgrade compatibility
- ✅ 45 GB freed via Disk Cleanup; 222.60 GB total usable space confirmed
- ✅ Ethernet connection verified stable
- ✅ System restore point created: "Before Windows 11 Upgrade"
- ✅ BitLocker suspended pre-upgrade: `manage-bde -protectors -disable C:`
- ✅ Third-party security software temporarily disabled

---

## Implementation & Procedures

### Phase 1: Pre-Upgrade System Verification

#### 1.1 TPM 2.0 Verification
```
Path: Settings > System > Device Security > Security Processor Details
Result: TPM 2.0 enabled
Alternative access: tpm.msc
```

#### 1.2 BitLocker Status Check
```powershell
manage-bde -status

# Output:
# Drive C: Conversion Status: Fully Decrypted
# Percentage Encrypted: 0.0%
# Protection Status: Protection Off
# (Suspended as planned before upgrade)
```

**Recovery Key Backup Locations:**
- Location 1: Microsoft Account (`account.microsoft.com/devices/recoverykey`)
- Location 2: USB Flash Drive (labeled "Windows 11 Recovery Keys")
- Location 3: Written on paper, stored physically

#### 1.3 System Backup
```
External Drive:
  Destination: Seagate 1TB
  Data: Documents, Projects, Downloads (156 GB)
  Duration: ~45 minutes
  Verification: Spot-checked 5 random files

Cloud (Google Drive):
  Cybersecurity Projects: 12.5 GB
  Duration: ~2 hours
  Status: All files accessible and verified
```

#### 1.4 Driver Pre-identification
```
GPU:     Nvidia 546.29
Network: Realtek RTL8111H
Chipset: Intel Chipset
Audio:   Realtek HD Audio
```

---

### Phase 2: Upgrade Execution

#### 2.1 Installation Details
```
Method:      Windows 11 Installation Assistant (Microsoft official)
Source:      microsoft.com/software-download/windows11
Start:       11:30 PM, November 27, 2025
Completed:   2:47 AM, November 28, 2025
Duration:    3 hours 17 minutes
Restarts:    3
```

**Upgrade Timeline:**
```
23:30 – Installation Assistant launched
23:35 – Compatibility check passed
23:40 – License accepted; installation started
23:45 – Phase 1: Preparing installation files
00:15 – First restart
00:30 – Phase 2: Installing system components
01:15 – Second restart
01:30 – Phase 3: Configuring settings and drivers
02:30 – Third restart (final)
02:47 – Login screen — installation complete
```

> Note: Phase 3 completed autonomously while the machine was unattended overnight.

#### 2.2 Post-Upgrade Immediate Verification
```
- System booted to Windows 11 login successfully
- All personal files present on Desktop, Documents, Downloads
- Critical applications intact: VS Code, VirtualBox, Kali Linux VM, Discord, Google Drive
- Windows 11 Build: 22621.3737
- TPM 2.0: Still enabled
- BitLocker: Still suspended (expected — manual re-enablement required)
```

---

### Phase 3: Post-Upgrade Security Hardening

#### 3.1 Windows Update
```powershell
# Run 1: 47 updates (~45 minutes)
# Run 2: 12 additional updates (~30 minutes)
# Run 3: "Your device is up to date"
# Total: 59 updates installed

# OEM Drivers Updated:
# GPU:     Nvidia 550.76
# Network: Realtek 10.05
# Chipset: Intel 2.8.1
# Audio:   Realtek HD Audio (latest)
```

#### 3.2 BitLocker Re-enablement
```powershell
manage-bde -protectors -enable C:

# Verification:
# Conversion Status: Fully Encrypted
# Percentage Encrypted: 100%
# Protection Status: Protection On
# Key Protectors: TPM, Recovery Key
```

#### 3.3 Privacy & Telemetry Hardening

**GUI Configuration**
```
Settings > Privacy & Security > General:
  - Tailored experiences: OFF
  - Improve inking & typing: OFF
  - Tailored suggestions: OFF
  - Advertising ID: OFF
  - Cloud content suggestions: OFF

Settings > Privacy & Security > Diagnostics & Feedback:
  - Diagnostic data: "Required diagnostic data" (minimum)
  - Optional diagnostic data: OFF
  - Tailored experiences: OFF
  - Feedback notifications: OFF
```

**Registry-Based Telemetry Reduction**
```powershell
# Run as Administrator

reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\DataCollection" /v AllowTelemetry /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\DataCollection" /v DoNotShowFeedbackNotifications /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\CloudContent" /v DisableWindowsConsumerFeatures /t REG_DWORD /d 1 /f

sc config DiagTrack start= disabled
sc stop DiagTrack

sc config dmwappushservice start= disabled
sc stop dmwappushservice
```

#### 3.4 UAC Hardening

```powershell
# Run as Administrator

# Set UAC to highest level (prompt on Secure Desktop)
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" /v ConsentPromptBehaviorAdmin /t REG_DWORD /d 2 /f
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" /v EnableInstallerDetection /t REG_DWORD /d 1 /f
```

**Alternative GUI Access (if Settings app doesn't show UAC):**
```
Run: UserAccountControlSettings.exe
Set slider to: "Always notify" (top position)
```

**Verification:**
Any attempt to run an elevated process now prompts a credential dialog on the Secure Desktop before proceeding.

#### 3.5 Account Privilege Separation

A standard user account (`DailyUser`) was created for all daily tasks. The administrator account is reserved strictly for system administration. This enforces least privilege — malware executing under the standard account cannot escalate without an explicit credential prompt.

#### 3.6 DNS Configuration

```
Previous: ISP default (203.118.x.x)
Primary:  Cloudflare 1.1.1.1
Secondary: Cloudflare 1.0.0.1

Path: Settings > Network & Internet > [Active Connection] > DNS Settings
Verification: ping 1.1.1.1
```

Cloudflare was selected over Google (8.8.8.8) and Quad9 (9.9.9.9) based on latency testing and privacy policy.

#### 3.7 Storage Configuration

**Storage Sense:**
```
Settings > System > Storage > Storage Sense
- Cleanup frequency: Daily
- Delete old Downloads: After 30 days
- Empty Recycle Bin: After 30 days
```

**Virtual Memory:**
```
RAM: 16 GB
Virtual Memory (pagefile): 32 GB on C:
Path: Control Panel > System Properties > Advanced > Virtual Memory
```

#### 3.8 Bloatware Removal

```powershell
# Run as Administrator

Get-AppxPackage -AllUsers | Where-Object {$_.Name -like "*3dbuilder*"} | Remove-AppxPackage
Get-AppxPackage -AllUsers | Where-Object {$_.Name -like "*bingnews*"} | Remove-AppxPackage
Get-AppxPackage -AllUsers | Where-Object {$_.Name -like "*bingweather*"} | Remove-AppxPackage
Get-AppxPackage -AllUsers | Where-Object {$_.Name -like "*GetStarted*"} | Remove-AppxPackage
```

**Removed via Settings > Apps:**
Calculator, Camera, Cortana, Feedback Hub, Mail & Calendar, Maps, Media Player, Microsoft 365 (trial), News, Outlook (pre-release), Photos (UWP), Power Automate, Quick Assist, Spotify (pre-install), Sticky Notes, Weather, Windows Clock, OneNote (UWP), Clipchamp.

**Retained:**
VS Code, Discord, OneDrive, Windows Terminal, Notepad, File Explorer, Microsoft Store.

#### 3.9 Additional Privacy Controls
```
Settings > Privacy & Security > Location:
  - Location: OFF
  - Location history: Cleared
  - Per-app permissions: Disabled

Settings > Privacy & Security > Activity History:
  - "Store my activity history on this device": Unchecked
```

#### 3.10 Post-Hardening References

- **Win11Debloat:** [Raphire/Win11Debloat](https://github.com/Raphire/Win11Debloat) — reviewed for additional removal options.
- **Network Optimization:** Khorvie Tech, *"The ONLY Network Optimization Guide You'll Ever Need"* — reviewed QoS settings and adapter power management.
- **Windows Optimization:** Khorvie Tech, *"The ONLY Windows PC OPTIMIZATION Guide You Will EVER Need In 2024"* — reviewed CPU unparking, service management, and power plan settings.

---

## Challenges Encountered & Solutions

### Challenge 1: BitLocker Unavailable After Initial Upgrade

**Problem:** After the upgrade completed, there was no BitLocker option in the Settings app.

**Root Cause:** The upgrade had been targeting Windows 11 Home, not Pro. BitLocker and EFS are Pro/Enterprise-only features. This was not flagged during the installation process.

**Solution:** Researched Windows edition feature differences, determined Pro was the correct target, and purchased the Windows 11 Pro license upgrade ($99–120) through the Microsoft Store. This unlocked BitLocker and Group Policy access immediately without requiring a reinstall.

**Outcome:** BitLocker enabled, 100% encrypted, recovery keys secured in three locations.

**Takeaway:** Verify target edition feature sets before starting an OS migration. The cost of getting it wrong is either wasted time or unplanned licensing spend.

---

### Challenge 2: UAC Settings Not Visible in Settings App

**Problem:** Following Security+ study notes to configure UAC via `Settings > Privacy & Security` produced no UAC option.

**Root Cause:** Windows 11 Home has limited Group Policy access, and some security controls only appear in the legacy Control Panel interface, not the modern Settings app.

**Solution:**
```
Method 1 (Used): Run UserAccountControlSettings.exe directly
  → Accessed the legacy slider interface
  → Set UAC to "Always notify"

Method 2 (Registry fallback):
  → reg add "HKLM\...\Policies\System" /v ConsentPromptBehaviorAdmin /t REG_DWORD /d 2 /f
  → Confirmed working via privilege escalation prompt test
```

**Outcome:** UAC set to maximum; Secure Desktop prompts confirmed functional.

**Takeaway:** The same security setting often has multiple access paths — modern GUI, legacy Control Panel, and registry. When one path fails, the others usually still work. Knowing all three is the difference between being blocked and moving forward.

---

### Challenge 3: BitLocker Showing "Fully Decrypted" Post-Upgrade

**Problem:** Running `manage-bde -status` after the upgrade showed `Protection Status: Protection Off` and `0.0%` encrypted.

**Root Cause:** BitLocker was intentionally suspended before the upgrade (`manage-bde -protectors -disable C:`). It does not re-enable automatically after an upgrade completes — manual re-enablement is required.

**Solution:**
```powershell
manage-bde -protectors -enable C:
manage-bde -status  # Verified: Fully Encrypted, Protection On
```

**Takeaway:** Suspended services do not resume themselves. Any security control that was paused pre-maintenance needs to be explicitly re-enabled and verified post-maintenance.

---

### Challenge 4: Post-Upgrade Performance Degradation

**Problem:** System was noticeably slower after the upgrade; Event Viewer showed multiple GPU-related warnings.

**Root Cause:** The Windows 11 installer deploys generic display drivers. The pre-downloaded Nvidia 546.29 was also no longer the latest build, and the old Windows 10 driver was partially incompatible.

**Solution:**
- Downloaded Nvidia 550.76 directly from nvidia.com (not via Windows Update)
- Performed a clean driver install (selected "clean installation" option in the installer)
- Updated Realtek, Intel Chipset, and audio drivers from their respective vendor pages

**Outcome:** Event Viewer warnings cleared; VirtualBox VMs and GPU workloads running normally.

**Takeaway:** Windows Update does not always deliver the latest or most stable OEM drivers. Going directly to the vendor page is usually the right call, especially for GPU and network adapters.

---

### Challenge 5: Recovery Key Not Appearing in Microsoft Account

**Problem:** After enabling BitLocker, the recovery key did not appear at `account.microsoft.com/devices/recoverykey`.

**Root Cause:** During the BitLocker setup flow, only the "Save to a file" and "Print the recovery key" options were selected. The Microsoft Account backup requires explicitly selecting "Save to your Microsoft account" during setup — it is not automatic.

**Solution:** Opened BitLocker management, selected "Back up your recovery key," and chose the Microsoft Account option. Key appeared in the portal within a few minutes.

**Outcome:** Recovery key confirmed accessible via Microsoft Account.

**Takeaway:** Read every line of the setup wizard. Skimming past backup options for a recovery key is the kind of mistake that causes a complete data lockout.

---

## Post-Implementation Review & Lessons Learned

### 1. Objectives vs. Results

| Objective | Target | Result | Status |
|-----------|--------|--------|--------|
| Upgrade to Pro edition | Enable BitLocker/EFS | Completed | ✅ |
| Full-disk encryption | BitLocker 100% | 100% encrypted | ✅ |
| Recovery key redundancy | 2+ locations | 3 locations (Microsoft, USB, paper) | ✅ |
| Security hardening | UAC + telemetry | All controls applied | ✅ |
| Downtime | Under 4 hours | 3 hours 17 minutes | ✅ |
| Data integrity | 100% file retention | All files intact | ✅ |

---

### 2. Security+ Domains — Practical Coverage

#### Domain 1: Threats, Attacks & Vulnerabilities
- Applied: Identified EOL risk (unpatched CVEs with no vendor fix path) and migrated proactively before the October 14, 2025 deadline.

#### Domain 2: Architecture & Design
- Applied: Principle of least privilege via account separation; defense-in-depth through layered controls (encryption, UAC, DNS, telemetry blocking).

#### Domain 3: Implementation
- Applied: BitLocker AES-256 encryption with TPM 2.0; UAC at maximum; account access control; DNS hardening; telemetry service disabling.

#### Domain 4: Operations & Incident Response
- Applied: Formal change management, pre/post checklists, rollback window, backup verification, Event Viewer monitoring post-change.

#### Domain 5: Governance, Risk & Compliance
- Applied: Risk assessment table; vendor lifecycle management (Windows support timeline); reproducible procedure documentation.

---

### 3. Technical Knowledge Gained

**Windows OS and Security Architecture**
- NTFS permissions and ACL structure
- Registry hive layout and policy-based security management
- Service management: stopping and disabling non-essential background processes
- Kernel vs. user-mode driver behavior and failure modes

**Encryption and Key Management**
- BitLocker: AES-256 with TPM 2.0 key sealing
- Key escrow: why multiple backup locations matter, and what happens if a recovery key is only in one place
- EFS: file-level encryption as a complement to full-disk encryption
- The difference between suspended protection and decrypted state

**Change Management**
- Pre-change risk assessment with defined rollback criteria
- Why checklists are not optional for destructive operations
- Post-change verification cadence (immediate, 24-hour, one-week)

---

### 4. What Was Done Right

- Maintained multiple independent backup methods and verified them before starting.
- Scheduled the upgrade overnight to minimize operational impact.
- Did not skip post-upgrade patching — 59 updates applied before any hardening work.
- Sourced OEM drivers directly from vendor pages rather than relying on Windows Update.
- Documented every issue and its resolution in real time rather than reconstructing from memory.

---

### 5. What Would Be Done Differently

- Verify target Windows edition feature requirements before beginning the upgrade, not during.
- Read the full BitLocker backup wizard rather than selecting the most obvious options.
- Pre-stage the Pro license upgrade as part of planning, not as a reactive fix during the process.
- Store the recovery key in a proper encrypted vault (Bitwarden, KeePassXC) rather than a social media DM. A messaging platform you don't control is not a secure key store — if the account is compromised, so is the key. This is the one part of the backup strategy that does not meet a real security standard.

---

## Security+ Concepts Applied

| Domain | Concepts Demonstrated |
|--------|-----------------------|
| 1: Threats, Attacks & Vulnerabilities | EOL risk, unpatched vulnerability exposure, proactive risk mitigation |
| 2: Architecture & Design | Least privilege, defense-in-depth, attack surface reduction |
| 3: Implementation | BitLocker (AES-256), UAC, account separation, DNS hardening, service disabling |
| 4: Operations & Incident Response | Change management, backup/recovery procedures, post-change monitoring |
| 5: Governance, Risk & Compliance | Risk assessment, documentation standards, vendor lifecycle management |

---

## Performance Metrics & Verification

### Baseline Comparison

| Metric | Pre-Upgrade (Win10 Home) | Post-Upgrade (Win11 Pro) |
|--------|--------------------------|--------------------------|
| Boot Time | ~25 seconds | ~18 seconds |
| Idle RAM | 3.8 GB / 16 GB | 4.2 GB / 16 GB |
| Idle CPU | 3–5% | 2–5% |
| Disk Used | ~78% (222 GB) | ~62% (post-debloat) |
| Telemetry Services Running | 5 | 0 |
| Full-Disk Encryption | None | BitLocker 100% |
| UAC Level | Default | Maximum (Secure Desktop) |

### 24-Hour Post-Upgrade Verification

**Performance:**
- Boot time: 18 seconds
- Idle RAM: 4.2 GB (expected for Windows 11 baseline)
- Idle CPU: 2–5%
- GPU load test: clean

**Security State:**
- Windows Defender: Running, real-time protection enabled
- Firewall: All profiles active
- BitLocker: 100% encrypted, Protection On
- UAC: Maximum (verified via privilege escalation test)
- Automatic updates: Enabled

**Application Compatibility:**
- VS Code: Functional
- VirtualBox + Kali Linux VM: Boots normally
- Discord (2FA): Working
- Chrome / Firefox: Working
- OneDrive sync: Working

**Event Viewer (24-hour window):**
- Critical errors: 0
- Warnings: 3 (GPU driver-related, non-critical, resolved after Nvidia 550.76 install)
- Informational events: 47 (normal system activity)

---

## References & Resources

### Official Microsoft Documentation
- [Windows 10 End of Support](https://support.microsoft.com/en-us/windows/windows-10-support-ends-on-october-14-2025)
- [Windows 11 System Requirements](https://learn.microsoft.com/en-us/windows/whats-new/windows-11-requirements)
- [BitLocker Overview](https://learn.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-overview)
- [UAC Settings and Configuration](https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/settings-and-configuration)
- [Windows 11 Privacy Settings](https://learn.microsoft.com/en-us/windows/privacy/windows-11-privacy)

### Community Resources

- [Raphire/Win11Debloat](https://github.com/Raphire/Win11Debloat) — automated script for bloatware removal, telemetry disabling, and privacy hardening. Reviewed for additional removal options beyond what was done manually.
- Khorvie Tech, *"The ONLY Network Optimization Guide You'll Ever Need"* — DNS optimization, QoS, network adapter power management, advanced adapter settings. [YouTube](https://www.youtube.com/watch?v=ZW54qe6-cKA) (Aug 26, 2025)
- Khorvie Tech, *"The ONLY Windows PC OPTIMIZATION Guide You Will EVER Need In 2024"* — power plan optimization, CPU core unparking, service disabling, registry tweaks, GPU driver optimization. [YouTube](https://www.youtube.com/watch?v=iBiNfa32AnE) (Apr 11, 2024)

### Personal Repository
- [uriel0byte/Cybersecurity-Writeups](https://github.com/uriel0byte/Cybersecurity-Writeups)

---

## Appendix: Quick Reference Commands

### BitLocker
```powershell
manage-bde -status                        # Check encryption status
manage-bde -protectors -disable C:        # Suspend before major changes
manage-bde -protectors -enable C:         # Re-enable after changes

# Backup recovery key to a file (save to external drive or USB)
manage-bde -protectors -get C: -Type RecoveryPassword > C:\RecoveryKey_Backup.txt
```

### UAC
```powershell
# Set to maximum (prompt on Secure Desktop)
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" /v ConsentPromptBehaviorAdmin /t REG_DWORD /d 2 /f
```

### Telemetry Services
```powershell

sc config dmwappushservice start= disabled && sc stop dmwappushservice
```

### Bloatware (PowerShell)
```powershell
# Pattern: Get-AppxPackage -AllUsers | Where-Object {$_.Name -like "*appname*"} | Remove-AppxPackage
```

---

**Document Version:** 1.1  
**Last Updated:** November 28, 2025   
**Author:** Supawat Huncharoen (uriel0byte)  
**Status:** Draft — pending final review before GitHub publish
