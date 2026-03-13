# Final Dashboard – SOC Threat Monitoring

**Built in:** Splunk Enterprise  
**Monitoring Period:** Last 30 days  

---

## Dashboard Panels

### Panel 1 — Brute Force Attempt Detection
```
index=wineventlog EventCode=4625
| stats count AS Failed_Attempts by Account_Name, Source_Network_Address
| where Failed_Attempts > 5
```

### Panel 2 — Network-Based Attack Activity (SYN Flood)
```
index=wineventlog (EventCode=4624 OR EventCode=4625)
| stats count by Source_Network_Address
| sort - count
```

### Panel 3 — Suspicious Process Execution Detection
```
index=wineventlog EventCode=4688
| stats count by New_Process_Name
| sort - count
```

---

## What the Dashboard Monitors
| Panel | Attack Type | Event ID |
|-------|------------|----------|
| Panel 1 | Brute Force | 4625 |
| Panel 2 | SYN Flood / Network Attack | 4624, 4625 |
| Panel 3 | Malware / Suspicious Process | 4688 |

---

## Outcome
Dashboard provides layered visibility across:
- Authentication attacks
- Network-level threats  
- Endpoint-level suspicious activity
