# Investigation Notes

## Incident Summary

A Registry Run Key was added under the current user's Run registry path.

This action represents a persistence mechanism frequently used by attackers to launch malware automatically after logon.

---

## Alert Source

Sysmon Event ID:

13

Registry Value Set

---

## MITRE ATT&CK

Technique:

T1547.001

Registry Run Keys / Startup Folder

Tactic:

Persistence

---

## Investigation Steps

### Verify Event

Filter:

```

data.win.system.eventID:13

```

---

### Review Event Fields

Important fields:

- Image
- TargetObject
- EventType
- Details
- User
- ProcessId
- ProcessGuid
- Timestamp

---

### Registry Path

```

HKCU\Software\Microsoft\Windows\CurrentVersion\Run

```

---

### Registry Value

Name

```

SOCLab

```

Value

```

notepad.exe

```

---

### Executing Process

PowerShell

created the registry entry.

---

### Expected Behavior

Legitimate software installers often modify Run Keys.

Unexpected additions may indicate malware persistence.

---

## Detection Timeline

Registry Value Created

↓

Sysmon Event ID 13

↓

Collected by Wazuh Agent

↓

Forwarded to Wazuh Manager

↓

Indexed

↓

Visible in Threat Hunting

---

## Findings

Registry modification successfully detected.

Event successfully ingested into Wazuh.

SOC analysts can identify:

- Registry path
- Executing process
- User
- Timestamp
- Registry value

---

## Analyst Verdict

Severity:

Medium

Reason:

Registry Run Key persistence is commonly used by malware to survive reboots and maintain execution.
