# GRC Report — Exercise 3: Firewall Hardening & Validation

- **Author:** Gene Crittenden
- **Date:** 2025-11-14
- **Environment:** VirtualBox Host-Only SOC Lab
- **Exercise:** Firewall Hardening, Nmap Reconnaissance & Wireshark Verification
- **Systems:**
  - Web Server — 192.168.56.101
  - Client/Scanner — 192.168.56.102

---

## Overview

This exercise demonstrates the implementation and validation of host-based firewall controls using UFW on a web server.
The goal from a GRC perspective is to ensure:

- Proper access control enforcement
- Auditability of firewall configuration
- Evidence collection to support compliance requirements
- Alignment with SOC and industry frameworks

---

## Scope

- Web server firewall configuration (`ufw default deny incoming`, allow/deny/reject rules)
- Network traffic validation using Nmap and Wireshark
- Lab environment: isolated host-only VirtualBox network
- Excluded: production network systems, internet-facing services

---

## Controls Implemented

| Control Type | Description | Implementation |
|--------------|-------------|----------------|
| Access Control | Restrict inbound network traffic to required ports only | UFW allow 80/tcp, allow 22/tcp |
| Port Restriction | Deny and reject non-essential services | UFW deny 443, reject 443 |
| Logging & Monitoring | Capture network traffic to verify control effectiveness | Wireshark packet captures exported as text |
| Evidence for Audit | Record of scans and firewall behavior | Nmap output, Wireshark screenshots, incident report |

---

## Compliance Mapping

| Framework | Control | Relevance |
|-----------|--------|----------|
| **NIST 800-53 (Rev. 5)** | AC-4 — Information Flow Enforcement | Firewall rules enforce access restrictions on network traffic |
| **ISO/IEC 27001:2022** | A.8.20 — Network Security | Demonstrates technical controls limiting exposure |
| **CIS Controls v8** | 4.1 — Establish and Maintain a Secure Configuration Process | Verification of firewall configuration and evidence collection |
| **PCI DSS v4.0** | 1.1.6 — Firewall / Router Configuration Standards | Ensures only authorized ports/services are exposed |

---

## Risk Assessment

- **Risk:** Unauthorized network access
**Mitigation:** Firewall DROP and REJECT rules
**Residual Risk:** Minimal — only allowed services exposed

- **Risk:** Misconfiguration of firewall rules
**Mitigation:** Nmap verification and Wireshark captures for validation
**Residual Risk:** Low — confirmed through testing

- **Risk:** Lack of audit evidence
**Mitigation:** Text exports of Wireshark captures and Nmap scans, incident report
**Residual Risk:** None — evidence properly documented

---

## Evidence Collected

| Evidence Type | File / Location | Purpose |
|---------------|----------------|---------|
| Firewall configuration | `firewall_rules.png` | Confirms implemented rules |
| Nmap scans | `nmap_scan_1.png`, `nmap_scan_2.png` | Demonstrates allowed/blocked ports |
| Packet captures | `baseline.txt` `firewall_blocking.txt` | Verifies firewall behavior |
| Screenshots | `wireshark_filters_1.png` `wireshark_filters_2.png` `wireshark_filters_3.png` `wireshark_filters_4.png` `wireshark_filters_5.png` | Visual documentation of allowed, dropped, and rejected traffic |
| Incident report | `incident_report.md` | SOC-style analysis of traffic and firewall behavior |

---

## Recommendations

1. Maintain explicit allow/deny lists and periodically review rules.
2. Forward firewall logs to a central SIEM for continuous monitoring.
3. Implement IDS/IPS (e.g., Suricata, Zeek) to detect scanning attempts.
4. Retain packet captures and scan evidence for audit purposes.
5. Include GRC alignment for future exercises: map each control to NIST/ISO/CIS standards.

---

## Conclusion

The firewall on **192.168.56.101** has been successfully configured, verified, and documented.
- Allowed services function correctly
- Unauthorized ports are either dropped silently or explicitly rejected
- Evidence (Nmap scans, Wireshark captures, screenshots, incident report) supports compliance and audit requirements

This exercise demonstrates **GRC alignment, risk mitigation, and SOC validation practices** in a controlled lab environment.

**End of Report**
