# Lab 01: Installing Sysmon and Analysing a Process Creation Event

A hands-on DFIR home-lab exercise: deploy Sysmon on a Windows 11 VM, generate activity, and read the evidence in Event Viewer.

## Objective

Learn how endpoint telemetry is collected and how to read a **Process Create (Event ID 1)** event the way a DFIR analyst would.

## Lab Environment

| Component | Details |
| --- | --- |
| Hypervisor | Oracle VirtualBox |
| Target VM | Windows 11 (Win11-Victim) |
| Other VM | Kali Linux |
| Tool | Sysmon v15.22 (Microsoft Sysinternals) |
| Config | [SwiftOnSecurity sysmon-config](https://github.com/SwiftOnSecurity/sysmon-config) |

> A clean VM snapshot was taken before the install, and another after ("Sysmon installed").

## What is Sysmon?

Sysmon (System Monitor) is a free Windows system service and driver that logs detailed activity to the Windows Event Log: process creation, network connections, file creation, and more. It records details that default Windows logs do not, such as parent process, command line, and file hashes.

## Setup Steps

1. Download Sysmon from the Microsoft Sysinternals page and extract it to `C:\Sysmon`.
2. Download `sysmonconfig-export.xml` (raw file) from the SwiftOnSecurity repository into the same folder.
3. Open PowerShell **as Administrator** and run each command separately:

```powershell
cd C:\Sysmon
.\Sysmon64.exe -accepteula -i sysmonconfig-export.xml
```

4. Confirm the output shows `Sysmon64 installed` and `Sysmon64 started`.

## Generating and Finding Activity

1. Open and close Notepad on the VM.
2. Open **Event Viewer** and go to:
   `Applications and Services Logs > Microsoft > Windows > Sysmon > Operational`
3. Locate the **Event ID 1** entry for `Notepad.exe`.


## Analysis

| Field | Value observed | Why it matters |
| --- | --- | --- |
| Event ID | 1 (Process Create) | Records every process that starts |
| Image | `...\WindowsNotepad...\Notepad.exe` | The program that ran and its path |
| CommandLine | Path to `Notepad.exe`, no extra arguments | Shows how it was launched |
| ParentImage | `C:\Windows\explorer.exe` | Started by Explorer, i.e. a normal user action |
| User | Local interactive user account | Who launched it |
| Hashes | MD5, SHA256, IMPHASH | Used to look up files in threat-intel databases |

**Finding:** the parent-child relationship (`explorer.exe` -> `Notepad.exe`) and the clean command line are consistent with normal user activity. In an investigation, an unexpected parent (for example an Office app spawning `powershell.exe`) would be a red flag.

## Troubleshooting Notes

- The config file appeared as "Microsoft Edge HTML Document" because Edge is the default handler for `.xml` files. Opening it in Notepad confirmed it was valid XML.
- `Sysmon64a.exe` is for ARM64 systems. `Sysmon64.exe` was used on this x64 VM.
- Running both commands on one line caused a PowerShell `ambiguous parameter` error. Running them separately fixed it.

## Skills Demonstrated

- Building and snapshotting a VM-based lab
- Deploying and configuring endpoint telemetry (Sysmon)
- Navigating Windows Event Viewer
- Interpreting process creation events (parent process, command line, hashes)
- Documenting troubleshooting steps

## Next Steps

- Compare parent processes for `cmd.exe` and `whoami.exe`
- Filter Event Viewer for Event ID 3 (network connections) and 11 (file creation)
- Analyse PCAP files in Wireshark
- Practise with Autopsy and FTK Imager on sample disk images

## Disclaimer

All activity was performed on a personal, isolated virtual machine for learning purposes.
