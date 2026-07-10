# Troubleshooting Notes

## Issue 1

No Sysmon Event ID 13 generated.

### Cause

Sysmon configuration does not include Registry Event monitoring.

### Resolution

Update the Sysmon configuration to monitor Registry Value Set events.

---

## Issue 2

No results in Wazuh Threat Hunting.

### Cause

Event not yet forwarded or indexed.

### Resolution

Wait 15–30 seconds and refresh the dashboard.

---

## Issue 3

Registry value not created.

### Cause

Incorrect registry path or PowerShell syntax.

### Resolution

Use the exact command:

```

New-ItemProperty `
-Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run" `
-Name "SOCLab" `
-Value "notepad.exe" `
-PropertyType String `
-Force

```

---

## Issue 4

Registry value not removed.

### Cause

Incorrect value name.

### Resolution

Verify the value using:

```

Get-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run"

```

Then remove it:

```

Remove-ItemProperty `
-Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run" `
-Name "SOCLab"

```

---

## Issue 5

No Event ID 13 in PowerShell.

### Cause

Sysmon service not running.

### Resolution

Verify:

```

Get-Service Sysmon64

```

---

## Issue 6

Wazuh Agent disconnected.

### Resolution

Verify:

```

Get-Service WazuhSvc

```

---

## Issue 7

Incorrect Wazuh filter.

### Correct Query

```

data.win.system.eventID:13

```

---

## Validation Checklist

✓ Sysmon service running

✓ Wazuh Agent running

✓ Registry Run Key created

✓ Event ID 13 generated

✓ Event visible in Wazuh

✓ JSON event reviewed

✓ Registry entry removed

✓ Lab cleanup completed
