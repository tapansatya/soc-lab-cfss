# Task 1 – Log Ingestion & Validation

**Objective:** Set up Sysmon and verify Windows Event Logs are forwarded to Splunk.

## Steps Completed
1. Downloaded Sysmon from Microsoft Sysinternals
2. Applied SwiftOnSecurity config (`sysmonconfig-export.xml`)
3. Installed Splunk Universal Forwarder on Windows 10 VM
4. Configured `inputs.conf` to forward Security, System, Application logs
5. Created `wineventlog` index in Splunk
6. Verified log ingestion via Splunk Search

## Splunk Queries Used

### Verify Sysmon logs
```
index=* sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational
```

### Verify login events
```
index=wineventlog EventCode=4624 
| table _time Account_Name IpAddress Logon_Type
```

## Key Event IDs Learned
| Event ID | Description |
|----------|-------------|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 1 | Sysmon - Process creation |

## Outcome
Successfully ingested 11,231+ events into Splunk. Confirmed logs are searchable and properly indexed.
