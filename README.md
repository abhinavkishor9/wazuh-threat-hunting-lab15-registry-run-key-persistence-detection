# wazuh-threat-hunting-lab15-registry-run-key-persistence-detection
## Overview

This lab demonstrates how attackers can establish persistence on Windows by modifying the Registry Run Key.

The activity is monitored using Sysmon Event ID 13 (Registry Value Set), collected by the Wazuh Agent, and investigated inside the Wazuh Dashboard Threat Hunting module.

The objective is to understand how registry-based persistence appears from a SOC analyst's perspective.

---

## Lab Objectives

- Understand Registry Run Key persistence
- Generate Sysmon Event ID 13
- Observe registry modifications in Wazuh
- Investigate event details
- Analyze JSON fields
- Clean up the persistence entry

---

## Lab Environment

| Component | Value |
|-----------|-------|
| Host OS | Windows 11 |
| SIEM | Wazuh 4.x |
| Agent | Wazuh Agent |
| Monitoring | Sysmon |
| Event ID | 13 |
| Technique | Registry Run Key Persistence |

---

## MITRE ATT&CK Mapping

| Tactic | Technique |
|---------|-----------|
| Persistence | T1547.001 – Registry Run Keys / Startup Folder |

---

## Steps Performed

### Step 1
Verified Sysmon and Wazuh services.

### Step 2
Created a Registry Run Key.

```

New-ItemProperty `
-Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run" `
-Name "SOCLab" `
-Value "notepad.exe" `
-PropertyType String `
-Force

```

### Step 3

Verified the registry entry.

```

Get-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run"

```

### Step 4

Confirmed Sysmon Event ID 13.

```

Get-WinEvent `
-LogName "Microsoft-Windows-Sysmon/Operational" `
-FilterXPath "*[System[(EventID=13)]]"

```

### Step 5

Opened Wazuh Dashboard.

Threat Hunting

↓

Filtered

```

data.win.system.eventID:13

```

### Step 6

Investigated:

- Process Name
- Registry Path
- Event Type
- User
- Image
- Timestamp

### Step 7

Viewed JSON event.

### Step 8

Removed the Registry Run Key.

```

Remove-ItemProperty `
-Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run" `
-Name "SOCLab"

```

---

## Detection Workflow

Registry Modification

↓

Sysmon Event ID 13

↓

Wazuh Agent

↓

Wazuh Manager

↓

Indexer

↓

Threat Hunting

↓

SOC Investigation

---


## Skills Learned

- Windows Persistence
- Registry Analysis
- Sysmon Event ID 13
- Wazuh Threat Hunting
- Event Investigation
- SOC Workflow
- MITRE ATT&CK Mapping

---

## Conclusion

Registry Run Keys are a common persistence mechanism used by attackers to execute programs automatically during user logon. This lab demonstrates how Sysmon captures registry modifications, how Wazuh ingests the events, and how SOC analysts can investigate registry-based persistence using Threat Hunting.
