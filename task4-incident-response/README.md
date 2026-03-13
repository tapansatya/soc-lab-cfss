# Task 4 – Incident Response & Playbook

**Objective:** Handle incidents like a real SOC analyst using NIST framework.

**Incident ID:** CFSS-SOC-IR-001  
**Incident Title:** Suspected Malware Execution on Windows Endpoint  
**Date & Time:** 25/01/2026 02:40:57 PM  
**Detected By:** SIEM Alert - Splunk  

---

## NIST Incident Response Framework

### Step 1 — Preparation ✅
Ensured environment was ready before incident occurred:
- SIEM configured (Splunk Enterprise)
- Sysmon enabled and running
- Logs forwarding active (Universal Forwarder)
- AV/Defender enabled on Windows VM

---

### Step 2 — Detection ✅
Identified brute force and network attacks via Splunk alerts.

**Indicators of Compromise found:**
- Suspicious process creation
- PowerShell execution with encoded commands
- Abnormal network connections

**Query used to find infected machine:**
```
index=wineventlog EventCode=4688
```
- EventCode 4688 = New process creation
- Identified which machine was compromised and attacker IP address

---

### Step 3 — Containment ✅
Stopped attack from Kali Linux spreading further.

**Actions taken:**
1. Isolated affected Windows VM
2. Disabled network adapter on compromised machine
3. Stopped malicious process via Task Manager
4. Added firewall rule to block attacker IP address

**Firewall rule added in Windows Defender:**
- Rule Name: `Block_Malicious_IP`
- Direction: Inbound + Outbound
- Action: Block
- Profile: All

---

### Step 4 — Eradication ✅
Verified no malicious services persisted after containment.

**Actions taken:**
1. Deleted malicious file from machine
2. Removed malicious Task/Service via Task Manager
3. Ran full AV scan using Windows Defender
   - Result: 0 threats found across 30,905 files scanned

---

### Step 5 — Recovery ✅
Confirmed system stability before bringing back online.

**Actions taken:**
1. Restored system to normal state
2. Re-enabled network adapter
3. Monitored logs for 24 hours via Splunk

**Verification Query:**
```
index=wineventlog EventCode=4688
```
- Result: 0 suspicious events in monitoring window ✅
- Confirmed system stability and successful recovery

---

## Incident Analysis Summary

| Field | Details |
|-------|---------|
| Attack Type | Brute Force + Malware Execution |
| Attack Source | Kali Linux VM (10.0.2.15) |
| Target | Windows 10 VM |
| Detection Method | Splunk SIEM Alert |
| Time to Detect | Within monitoring window |
| Containment Method | Firewall block + network isolation |
| Outcome | Successfully contained and recovered |

---

## Key Lessons Learned
- Continuous log monitoring is critical for early threat detection
- Prompt containment prevented potential lateral movement
- Stronger alert thresholds will improve future response time
- Endpoint monitoring (Sysmon) provided crucial forensic evidence
