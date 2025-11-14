# Firewall-Hardening-Nmap-Scanning-Wireshark-Analysis
This exercise demonstrates how to validate firewall rules using UFW, Nmap, and Wireshark inside a controlled SOC lab environment.

- **Author:** Gene Crittenden
- **Lab Environment:** VirtualBox (Host-Only Network)
- **Web Server:** 192.168.56.101
- **Client/Scanner:** 192.168.56.102

---

# Overview

This exercise demonstrates how to validate firewall rules using **UFW**, **Nmap**, and **Wireshark** inside a controlled SOC lab environment.
The goal is to show how a SOC analyst confirms that:

- Allowed services respond normally
- Blocked ports drop traffic silently
- Rejected ports actively return ICMP/TCP error messages
- Network captures can be used as evidence of security control behavior

This exercise is part of a broader SOC/GRC home-lab designed for hands-on practice and GitHub documentation.

---

# Lab Setup

### **Virtual Machines**
| Role | IP Address | Purpose |
|------|------------|---------|
| Web Server | 192.168.56.101 | nginx + UFW firewall |
| Client/Scanner | 192.168.56.102 | Nmap + Wireshark |

### **Installed Tools**
- nginx (web server)
- ufw (firewall)
- nmap (scanner)
- wireshark/tshark (packet capture & analysis)

### **Network**
VirtualBox **Host-Only** network: isolated from the internet, safe for scanning.

---

## 🔧 Firewall Configuration (Web Server)

The following rules were used to test both **DROP** and **REJECT** actions:

- sudo ufw default deny incoming
- sudo ufw allow 80/tcp
- sudo ufw allow 22/tcp # optional
- sudo ufw deny 443
- sudo ufw reject 443
- sudo ufw enable
- sudo ufw status verbose

| Port | Status |
|------|------------|
| Port 80 | Allowed (nginx) |
| Port 22 | Allowed (optional) |
| Port 443 | Intentionally set to Deny and Reject |
| All others | Silently dropped |

---

# **Nmap Tests from Client**
1. Baseline Scan before firewall rules
- nmap 192.168.56.101
2. Firewall validation scan
- nmap -sS -Pn 192.168.56.101
3. Targeted scan fore Port 443
- nmap -sS -p 443 192.168.56.101

Observations:
- Port 80 responds normally
- Port 443 appears as closed/filtered
- Oter ports sow as filtered (silent drop)

# **Wireshark Analysis on Web Server**

Wireshark was used to capture and verify the firewall behaviour. Filters used:
1. Allowed traffic on Port 80
- tcp.port == 80
2. Dropped traffic (silent drop/SYN retransmission)
- tcp.flags.syn == 1 && ip.src == 192.168.56.102
3. Rejected traffic (ICMPv6 admin prohibited on port 443)
- icmpv6
4 . Rejected traffic (TCP reset)
- tcp.flags.resent == 1
5. General communication between hosts
- ip.addr == 192.168.56.101 || ip.addr == 192.168.56.102

--- 

# Supporting Files

| File | Description |
|------|------------|
| baseline.txt | Text file of packet capture pre firewall rules |
| firewall_blocking.txt  | Text file of packet capture after firewall rules set and client performs scans |
| wireshark_filters_1.png | Screenshot of wireshark filters used |
| wireshark_filters_2.png | Screenshot of wireshark filters used |
| wireshark_filters_3.png | Screenshot of wireshark filters used |
| wireshark_filters_4.png | Screenshot of wireshark filters used |
| wireshark_filters_5.png | Screenshot of wireshark filters used |
| nmap_scan_1.png | Screenshot of Nmap scans from Client |
| nmap_scan_2.png | Screenshot of Nmap scans from Client |

The original .pcapng files are kept offline and available upon request

--- 

# Goals for this Exercise

**SOC Skills**
- Validate firewall configurations
- Capture and analyse network traffic
- Distinguish DROP vs REJECT behaviour
- Understand TCP retransmissions and ICMP errors
- Document findings professionally

**GRC Skills**
- Demonstrate that controls are properly implementing
- Produce evidence for audits or compliance
- Show how technical controls support policy requirements

---

# Conclusion

This exercise condirms that the firewall on 192.168.56.101 is working correctly:
-  Allowed services (HTTP) are reachable
-  Unauthorised ports are either silenty dropped or explicitly rejected
-  Network captures support the expected behaviour
-  Documentation and artifacts show a complete SOC investigation workflow
