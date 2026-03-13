# MITRE ATT&CK Mapping – CFSS SOC Lab

This file maps every attack simulated in this lab to the official 
MITRE ATT&CK framework used by real SOC analysts worldwide.

---

## 🔴 Attacks Simulated vs MITRE Techniques

| # | Attack Simulated | MITRE Tactic | Technique ID | Technique Name |
|---|-----------------|--------------|--------------|----------------|
| 1 | Nmap SYN Scan | Reconnaissance | T1046 | Network Service Discovery |
| 2 | SYN Flood (hping3) | Impact | T1498 | Network Denial of Service |
| 3 | Brute Force Login | Credential Access | T1110 | Brute Force |
| 4 | Scheduled Task Creation | Persistence | T1053.005 | Scheduled Task |
| 5 | New Service Creation | Persistence | T1543.003 | Windows Service |
| 6 | Encoded PowerShell | Defense Evasion | T1027 | Obfuscated Files or Information |
| 7 | PowerShell Execution | Execution | T1059.001 | PowerShell |
| 8 | Remote File Download | Command & Control | T1105 | Ingress Tool Transfer |
| 9 | Process Creation Monitoring | Discovery | T1057 | Process Discovery |

---

## 🔵 Detections Built vs MITRE Techniques

| Detection | Splunk Query / Tool | MITRE Technique Detected |
|-----------|-------------------|--------------------------|
| Failed logins > 5 | EventCode=4625 | T1110 – Brute Force |
| New service installed | EventCode=7045 | T1543.003 – Windows Service |
| Scheduled task created | EventCode=4698 | T1053.005 – Scheduled Task |
| PowerShell execution | EventCode=1 + Image=powershell.exe | T1059.001 – PowerShell |
| Encoded command decode | CyberChef Base64 | T1027 – Obfuscated Files |
| Hash lookup | VirusTotal SHA256 | T1204 – User Execution |
| Process creation | EventCode=4688 | T1057 – Process Discovery |
| SYN Flood packets | Wireshark tcp.flags.syn==1 | T1498 – Network DoS |

---

## 🟡 MITRE ATT&CK Tactics Covered
```
Initial Access → Execution → Persistence → 
Defense Evasion → Credential Access → Discovery → 
Command & Control → Impact
```

| Tactic | Coverage in This Lab |
|--------|---------------------|
| Reconnaissance | ✅ Nmap scanning |
| Execution | ✅ PowerShell commands |
| Persistence | ✅ Scheduled tasks, services |
| Defense Evasion | ✅ Encoded PowerShell |
| Credential Access | ✅ Brute force simulation |
| Discovery | ✅ Process + network discovery |
| Command & Control | ✅ Remote file download via PS |
| Impact | ✅ SYN Flood DoS simulation |

---

## 📚 References
- MITRE ATT&CK Framework: https://attack.mitre.org
- Sysmon Event IDs: https://github.com/SwiftOnSecurity/sysmon-config
- Splunk Security Essentials: https://splunkbase.splunk.com
    
