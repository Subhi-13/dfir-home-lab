# DFIR Home Lab

A hands-on lab log from my journey towards becoming a DFIR (Digital Forensics and Incident Response) analyst. Each day I build or test something in a safe virtual lab, record what I did, and write down what I learned.

## Lab Setup
- **Host:** my laptop running VirtualBox
- **Victim machine:** Windows 11 VM (`Win11-Victim`)
- **Attacker / tools machine:** Kali Linux VM
- **Monitoring tool:** Sysmon with the SwiftOnSecurity config

## Daily Log
| Day | Topic | Write-up | Screenshots |
|---|---|---|---|
| 1 | Installing Sysmon and reading my first event (Event ID 1) | [DAY-1 README](DAY-1%20README.md) | [dfir journey day 1 screenshots](dfir%20journey%20day%201%20screenshots) |
| 2 | Tracing processes with Sysmon ParentImage | [DAY-2 README](DAY-2%20README.md) | [2nd day lab screenshots](2nd%20day%20lab%20screenshots) |

## What I'm Learning
- Reading Sysmon events in Windows Event Viewer
- Spotting normal vs suspicious parent-child process relationships
- Building a safe, snapshot-protected lab for experiments

## Up Next
- Install SIFT Workstation and set up a host-only network
- Practise Event Viewer filters for other Sysmon events (Event ID 3 and 11)
- Explore free forensic tools such as FTK Imager, Autopsy and Wireshark

## Note
All activity happens inside isolated virtual machines for learning purposes only.
