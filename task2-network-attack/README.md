# Task 2 – Network Attack Detection

**Objective:** Identify, monitor, and analyze suspicious network activities.

## Attack 1 — Nmap SYN Scan

**Tool:** Kali Linux → Nmap  
**Command used:**
```
nmap -sS -A <Target_IP>
```
**What it does:** Scans all ports on target Windows VM to find open services.

## Attack 2 — SYN Flood Attack

**Tool:** Kali Linux → hping3  
**Command used:**
```
sudo hping3 -S --flood -p 445 <Target_IP>
```
**What it does:** Floods target with SYN packets without completing TCP handshake (DoS simulation).  
**Observed:** 489,960,861 packets transmitted, 100% packet loss on target.

## Wireshark Analysis

**Filter used to detect SYN Flood:**
```
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

**What to look for:**
| Indicator | Meaning |
|-----------|---------|
| Repeated SYN packets (Flag 0x02) with no ACK | SYN Flood attack |
| Single source IP → many destination ports | Nmap port scan |
| High packet rate from one IP | DoS behavior |

## Key Learnings
- Identified attack patterns at TCP/IP level using Wireshark
- Distinguished between normal traffic and malicious scanning/flooding
- Extracted attacker IP and protocol behavior from packet captures

## Outcome
Successfully captured and analyzed 223,892 packets. Identified SYN Flood behavior 
using TCP flag filtering. Saved capture as `syn_flood_attack.pcapng`.
