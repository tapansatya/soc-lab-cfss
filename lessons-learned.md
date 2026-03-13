# Lessons Learned – CFSS SOC Analyst Internship

This document summarizes key technical and analytical lessons learned
during the internship. Written to reflect real SOC analyst thinking.

---

## 🔵 Task 1 – Log Ingestion & Validation

### What I learned:
- How SIEM pipelines work end-to-end:
  `Log Source → Forwarder → Indexer → Search`
- Sysmon generates far more detailed logs than default Windows logging
- `inputs.conf` controls exactly which logs get forwarded to Splunk
- Event IDs are the foundation of all SOC detection logic

### What surprised me:
- Default Windows logging misses critical events like process creation
- Sysmon Event ID 1 captures SHA256 hashes automatically — very useful for IOC hunting

### How I'd do it better:
- Enable audit policies on Windows before installing Sysmon
- Create separate Splunk indexes for each log type from the start

---

## 🟠 Task 2 – Network Attack Detection

### What I learned:
- Difference between Nmap reconnaissance and active DoS attacks
- SYN Flood works by exhausting TCP connection state tables
- Wireshark filters like `tcp.flags.syn==1 && tcp.flags.ack==0`
  can isolate attack traffic from normal traffic instantly
- Attacker IP and behavior patterns are visible in packet captures

### What surprised me:
- hping3 transmitted nearly 490 million packets in seconds —
  showing how devastating DoS attacks can be even in a lab
- Normal traffic and attack traffic look very different once you 
  know what TCP flags to look for

### How I'd do it better:
- Set up Snort or Suricata alongside Wireshark for automated detection
- Forward Wireshark pcap data into Splunk for unified analysis

---

## 🔴 Task 3 – Threat Hunting & Rule Creation

### What I learned:
- Threat hunting is proactive — you look for threats before alerts fire
- Encoded PowerShell is one of the most common attacker evasion techniques
- CyberChef is an essential tool for decoding obfuscated commands
- VirusTotal hash lookups can quickly validate if a file is known malicious
- Persistence mechanisms (services, scheduled tasks) survive reboots —
  attackers rely on this heavily

### What surprised me:
- Scheduled task logs were disabled by default — easy for attackers to exploit
- A 0/70 detection ratio on VirusTotal doesn't mean a file is safe —
  it could be a brand new or custom malware (zero-day)

### How I'd do it better:
- Create alerts for ALL persistence event IDs from day one
- Automate hash lookups using VirusTotal API inside Splunk

---

## 🟢 Task 4 – Incident Response & Playbook

### What I learned:
- NIST IR framework (Prepare → Detect → Contain → Eradicate → Recover)
  gives a clear structured approach to any incident
- Containment must happen BEFORE eradication — rushing to clean up
  before isolating can let attackers move laterally
- Firewall rules can block attacker IPs at OS level without needing
  external hardware
- Post-recovery monitoring is just as important as the response itself

### What surprised me:
- How fast an attacker can move once inside — lateral movement is real
- Disabling the network adapter immediately stops most attack vectors
- Windows Defender found 0 threats after manual cleanup —
  confirming eradication was successful

### How I'd do it better:
- Create a formal IR checklist before any simulation
- Document timestamps for every action taken (chain of custody)
- Set up automated isolation playbooks in Splunk SOAR

---

## 🏆 Overall Internship Takeaways

### Technical Skills Gained:
- Splunk SPL query writing for threat detection
- Windows Event Log analysis (Security, System, Sysmon)
- Network packet analysis with Wireshark
- Malware analysis basics (hash lookup, PowerShell decode)
- NIST Incident Response process hands-on

### Analyst Mindset Gained:
- Think like an attacker to detect like a defender
- Logs tell a story — learn to read that story chronologically
- Every alert needs context — not every alert is a real threat
- Speed of containment determines severity of impact

### What I want to learn next:
- SOAR (Security Orchestration, Automation and Response)
- Cloud SIEM (Microsoft Sentinel / AWS Security Hub)
- Malware reverse engineering basics
- Threat intelligence platforms (MISP, OpenCTI)

---

## 💬 One Line Summary
> *"This internship taught me that SOC work is not just about tools —
> it's about understanding attacker behavior, thinking critically 
> about every log, and responding with speed and precision."*
