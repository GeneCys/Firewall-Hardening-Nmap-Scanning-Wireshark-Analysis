# Firewall Hardening & Validation Lab

**SOC-style exercise** demonstrating firewall rule validation using UFW, Nmap, and Wireshark inside a controlled lab environment.

- **Author:** Gene Crittenden  
- **Lab Environment:** VirtualBox (Host-Only Network)  
- **Web Server VM:** `192.168.56.101` 
- **Client/Scanner VM:** `192.168.56.102`  

---

## Overview

This exercise demonstrates how to validate firewall rules using **UFW**, **Nmap**, and **Wireshark**. The goal is to show how a SOC analyst confirms that:

- Allowed services respond normally  
- Blocked ports drop traffic silently  
- Rejected ports actively return ICMP/TCP error messages  
- Network captures can serve as evidence of security control behavior  

This lab is part of a broader SOC/GRC home-lab designed for hands-on practice and professional GitHub documentation.

---

## Lab Setup

### Virtual Machines
| Role | IP Address | Purpose |
|------|------------|---------|
| Web Server | `192.168.56.101` | nginx + UFW firewall |
| Client/Scanner | `192.168.56.102` | Nmap + Wireshark |

### Installed Tools
- nginx (web server)  
- ufw (firewall)  
- nmap (scanner)  
- wireshark/tshark (packet capture & analysis)  

### Network
- VirtualBox **Host-Only Network**: isolated from the internet, safe for testing

---

## Firewall Configuration (Web Server)

The following rules were applied to test **DROP** and **REJECT** behavior:

<details><summary>UFW Commands</summary>

```bash
sudo ufw default deny incoming
sudo ufw allow 80/tcp
sudo ufw allow 22/tcp  # optional
sudo ufw deny 443
sudo ufw reject 443
sudo ufw enable
sudo ufw status verbose
```
</details>

---

## Nmap Tests (Client VM)

<details><summary>Nmap Commands</summary>

1. Baseline scan before firewall rules:
```bash
nmap 192.168.56.101
```

2. Firewall Validation Scan
```bash
nmap -sS -Pn 192.168.56.101
```

3. Targetted Scan of Port 443
```bash
nmap -sS -p 443 192.168.56.101
```
</details>

**Observations:**
- Port 80 responds normally
- Port 443 shows as closed/filtered
- Other ports appear filtered (silent drop)

---

## Wireshark Analysis (Web Server)

Packet captures were used to verify firewall behavior. Filters applied:

<details> <summary>Wireshark Filters</summary>

```wireshark
Allowed traffic (Port 80): tcp.port == 80
Dropped traffic (SYN retransmissions): tcp.flags.syn == 1 && ip.src == 192.168.56.102
Rejected traffic (ICMP error): icmpv6
Rejected traffic (TCP reset): tcp.flags.reset == 1
General communication: ip.addr == 192.168.56.101 || ip.addr == 192.168.56.102
```
</details>

---

## Skills Demonstrated
- Firewall validation using UFW
- Nmap scanning & port analysis
- Network traffic analysis with Wireshark
- SOC workflow: Detect → Verify firewall controls
- Troubleshooting firewall behavior and TCP/ICMP responses

---

## Lessons Learned
- Allowed, blocked, and rejected traffic behaves differently; verification requires both scanning and packet capture
- TCP retransmissions indicate silently dropped packets
- ICMP and TCP reset messages are evidence of rejected ports
- Documenting firewall behavior creates reproducible SOC evidence for GRC and audit purposes

---

## Supporting Files
**Reports**
- [Incident Report](./incident_report.md)
- [GRC Report](./grc_report.md)

[Logs](./logs)
- Nmap Scans & Wrieshark Packet Captures
  - _The original .pcapng file is kept offline and available upon request._

[Screenshots](./screenshots)
- Nmap Scans & Wireshark Filters

---

## Conclusion
This lab confirms that the firewall on `192.168.56.101` is functioning as expected:
- Allowed services (HTTP) are reachable
- Unauthorized ports are either silently dropped or explicitly rejected
- Packet captures validate firewall behavior
- Documentation demonstrates a complete SOC investigation workflow

--- 

**End of Report**
