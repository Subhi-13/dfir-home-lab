# Day 2: Sysmon Process Creation and ParentImage Analysis

## Objective
Generate known activity on a Windows 11 VM and trace it through Sysmon Event ID 1 (Process Create), focusing on the ParentImage field.

## Lab Setup
- Host: VirtualBox
- VM: Win11-Victim (Windows 11 Pro)
- Sysmon v15.22 with SwiftOnSecurity config
- Snapshot taken before testing: "Sysmon installed"

## Activity Performed
Ran `whoami`, `hostname` and `ipconfig` in Command Prompt, and opened Notepad.

## Findings
| Activity | Image | ParentImage |
|---|---|---|
| whoami / hostname / ipconfig | C:\Windows\System32\*.exe | cmd.exe |
| Notepad | notepad.exe | explorer.exe |

## Key Takeaway
ParentImage shows which process launched another one. Commands typed in cmd have `cmd.exe` as the parent, while apps opened from the GUI have `explorer.exe`. Unusual parent-child pairings (for example, Word spawning `cmd.exe` or PowerShell) are a common red flag in incident response.

## Screenshots
See the separate file: **2nd day lab screenshots**.

## Next Steps
Install SIFT Workstation and set up a host-only network.
