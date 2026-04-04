# AWS Honeynet — Weeks 3–5 Observation Report
**Observation Period:** December 15, 2025 – January 5, 2026

---

## Overview

This report summarizes findings from the AWS Honeynet project across weeks 3–5 of the observation period. The honeynet consists of multiple honeypot services — Dionaea, Honeytrap, Cowrie, Heralding, and Tanner — monitored using Suricata IDS and a suite of network analysis dashboards. The goal is to capture, analyze, and correlate real-world attack traffic to identify threat patterns, attacker behaviors, and infrastructure origins.

The observation window spanned three weeks over the holiday period, yielding some of the most analytically rich data of the project — including coordinated attack surges, NSA-grade exploit activity, and the dominant exploitation of a nearly 20-year-old vulnerability.

---

## Key Findings Summary

- **Three major attack spikes** were observed: December 17–18, December 29, and January 4–5, each correlating with distinct threat actors, honeypots, countries, and ports.
- **Holiday-period exploitation** was a defining theme — reduced defensive posture over Christmas and New Year's correlated directly with targeted remote desktop and credential-based attacks.
- **A 2006 RealVNC authentication bypass (CVE-2006-2369)** was the single most detected exploit, accounting for 114,571 Suricata CVE alerts — underscoring the persistent danger of unpatched legacy systems.
- **DoublePulsar backdoor communication** — the NSA-developed tool used in the 2017 WannaCry ransomware attack — was detected 21,858 times, indicating active exploitation of SMB vulnerabilities at scale.
- **Commercial cloud infrastructure abuse** was confirmed, with DigitalOcean and AWS appearing in the top 10 attacker ASNs.
- **80–85% of attacking IPs** were already flagged in threat intelligence databases, consistent with previous observation periods.

---

## Attack Timeline

### December 17–18 Spike
The first major spike was driven by **Honeytrap**, peaking at approximately 52,000 attacks. The dominant source country was **Indonesia**, traced back to the **FUTURE-LAIN ISP (ASN 36530)**. Suricata flagged active **Attempted Administrator Privilege Gain** alerts during this window, elevating the event from a volumetric anomaly to a confirmed active exploitation attempt. This aligns with broader global cyber threat activity observed mid-December 2025.

### December 29 Spike
The second spike occurred over the Christmas-to-New-Year holiday window — a well-documented period of elevated cyber risk due to reduced staffing and defensive coverage. **Dionaea** and **Heralding** both spiked, with **Port 5900 (VNC)** surging to approximately 38,000 attacks. This indicates deliberate hunting for remote desktop access on unattended systems. Source traffic was geographically distributed, consistent with opportunistic holiday-period exploitation.

### January 4–5 Spike
The most analytically complex event of the observation period produced **two simultaneous but independent attack surges**:

1. **Dionaea** spiked to ~65,000 attacks targeting SMB services (Port 445), likely representing Windows service exploitation campaigns.
2. **Cowrie** independently spiked, correlated with a massive surge in **Port 22 (SSH)** traffic and **Canada (ASN: various)** as the dominant source country — strongly suggesting a targeted SSH brute force campaign proxied through compromised Canadian infrastructure.

The simultaneous but independent nature of these spikes points to distinct threat actors or automated campaigns operating in parallel.

---

## Honeypot Analysis

| Honeypot | Primary Activity | Notable Spike |
|----------|-----------------|---------------|
| Honeytrap | Network service probing | Dec 17–18 (52k) |
| Dionaea | SMB/Windows exploitation | Jan 4–5 (65k) |
| Cowrie | SSH/Telnet brute force | Jan 4–5 (independent) |
| Heralding | Credential-based attacks | Dec 29 (contributing) |
| Tanner | Web application scanning | Low throughout |

---

## Geographic Analysis

### Baseline vs. Burst Attackers
A key finding across the observation period is the distinction between **sustained baseline attackers** and **burst campaign attackers**:

- **United States (~35–40% aggregate)** — maintained a consistently high baseline throughout, characteristic of large-scale automated scanning from US-based cloud and residential infrastructure rather than targeted campaigns.
- **Indonesia (~20% aggregate)** — produced a concentrated December 17–18 burst, traced to FUTURE-LAIN ISP, dominated by Port 445 SMB scanning.
- **Canada** — low baseline but massive January 4–5 burst, pushing Canada to third in aggregate volume. Strongly associated with proxied SSH brute force activity.

### Country Port Profiles
Different countries exhibited strikingly different attack profiles:
- **Indonesia, China, Italy** — nearly exclusively Port 445 (SMB), indicating single-purpose automated scanning tools.
- **United States** — most diverse profile, spanning VNC (5900), SSH (22), SMB (445), and additional ports, suggesting sophisticated multi-vector botnet infrastructure.
- **South Korea** — significant Telnet (Port 23) component alongside VNC and SMB, suggesting legacy infrastructure targeting.

### Attack Map
The dynamic attack map revealed **South America** as a significant collective attack source not fully captured in the top-5 country breakdown, likely driven by Brazil-based botnet infrastructure. Europe contributed widespread but diffuse low-volume scanning.

---

## Port & Protocol Analysis

| Port | Protocol | Notable Activity |
|------|----------|-----------------|
| 445 | SMB | Dominant throughout entire period |
| 5900 | VNC | Major Dec 29 spike (~38k) — holiday remote desktop hunting |
| 22 | SSH | Jan 4–5 spike — Canadian-proxied brute force campaign |
| 5920 | Unknown | Anomalous spike — possible C2 or non-standard VNC variant |
| 23 | Telnet | South Korea-origin legacy targeting |

Port 5920 represents the most anomalous finding — an uncommon, non-standard port appearing in the attack data, potentially indicating custom malware command-and-control communication or a misconfigured service variant. This warrants further investigation against threat intelligence feeds.

---

## Suricata IDS Analysis

### Alert Categories
- **Miscellaneous Activity** dominated as a consistent daily baseline throughout the period.
- **Attempted Administrator Privilege Gain** spiked sharply on December 17th and again on December 22nd, confirming that the mid-December event involved active exploitation attempts rather than passive reconnaissance.
- **Generic Protocol Command Decode** alerts co-occurred with privilege escalation attempts, suggesting protocol-level command injection techniques.

### Top CVEs Detected

| CVE | Count | Description |
|-----|-------|-------------|
| CVE-2006-2369 | 114,571 | RealVNC authentication bypass — 2006 |
| CVE-2025-55182 | 723 | React2Shell RCE in React/Next.js — Dec 2025 |
| CVE-2021-3449 | 227 | OpenSSL denial-of-service |
| CVE-2019-11500 | 211 | Dovecot mail server RCE |
| CVE-2002-0013/0012 | 145 | SNMP vulnerabilities — 2002 |
| CVE-2002-1149 | 114 | Legacy 2002 vulnerability |
| CVE-2024-4577 | 65 | PHP-CGI argument injection RCE |
| CVE-2023-46604 | 64 | Apache ActiveMQ RCE — used in ransomware |
| CVE-2021-41773 | 34 | Apache HTTP Server path traversal RCE |

**The most significant finding:** CVE-2006-2369, a RealVNC authentication bypass disclosed and patched in 2006, accounted for the vast majority of all CVE-triggered alerts. Attackers successfully identified VNC servers with no authentication enabled and repeatedly attempted to breach protected ones. The CVE data spans 2002 to 2025, confirming that attackers maintain active exploit toolkits across multiple decades — opportunistically targeting any unpatched system regardless of patch age.

### Top Alert Signatures

| Signature | Count | Significance |
|-----------|-------|-------------|
| GPL INFO VNC Server Response | 128,621 | VNC most probed service |
| ET EXPLOIT VNC Not Requiring Auth | 24,786 | CVE-2006-2369 confirmed exploitation |
| ET INFO VNC Authentication Failure | 24,771 | Persistent auth bypass attempts |
| ET EXPLOIT DoublePulsar Backdoor | 21,858 | NSA-grade backdoor installation attempts |
| ET DROP Dshield Block Listed | 17,353 | Known attacker IPs confirmed |
| ET SCAN NMAP | 5,165 | Active reconnaissance confirmed |

**DoublePulsar** — originally developed by the NSA's Equation Group and leaked by Shadow Brokers in 2017 — was detected attempting backdoor installation 21,858 times. DoublePulsar runs in kernel mode, granting full system control upon successful installation, and was the primary payload used in the global WannaCry ransomware attack. Its detection in the honeynet data directly corroborates the sustained SMB Port 445 dominance and Dionaea honeypot activity observed throughout the period.

---

## Attacker Infrastructure

### IP Reputation
Approximately **80–85% of attacking source IPs** were already flagged as known malicious actors in threat intelligence databases — consistent with ratios observed in previous observation periods. This stability in attacker composition, despite dramatic volumetric spikes, confirms that existing threat intelligence feeds would have proactively blocked the majority of observed traffic.

### Top ASNs

| ASN | Provider | Count | Notes |
|-----|----------|-------|-------|
| AS36530 | FUTURE-LAIN | 94,152 | Indonesian ISP — Dec 17–18 spike |
| AS14061 | DigitalOcean | 39,652 | Cloud infrastructure abuse |
| AS398019 | DYNU | 24,261 | Dynamic DNS abuse |
| AS3786 | LG DACOM | 19,617 | South Korean ISP |
| AS4134 | Chinanet | 14,743 | China state-owned ISP |
| AS3269 | TIM | 11,046 | Italian telecom |
| AS16509 | Amazon AWS | 8,057 | Cloud infrastructure abuse |

The presence of **DigitalOcean and AWS** in the top 10 ASNs confirms that commercial cloud infrastructure is actively being abused as attack launchpads — a critical finding for organizations that trust cloud-origin traffic by default.

### Top Source IPs
The top source IP (167.114.38.196) generated 60,072 connections alone — characteristic of automated botnet behavior. The top three IPs collectively account for a disproportionate share of total attack volume, suggesting that blocking a small number of high-volume source addresses would meaningfully reduce overall attack traffic.

---

## Credential Analysis

The username and password tag clouds revealed that the vast majority of credential-based attacks targeted **default and weak credentials**:

- **Most common usernames:** root, sa, admin, ubuntu, postgres, mysql, oracle — all default service account names
- **Most common passwords:** 123456, password, blank (no password), 12345678, admin123, qwerty
- **Notable:** A significant proportion of attacks attempted **blank passwords**, specifically hunting for misconfigured systems with no authentication — consistent with the CVE-2006-2369 exploitation pattern.

---

## Conclusions & Defensive Implications

1. **Legacy vulnerabilities remain the primary attack surface.** A 2006 vulnerability was the most exploited finding of the entire period. Organizations must prioritize patching regardless of patch age.

2. **Holiday periods represent elevated risk windows.** The December 29 VNC hunting campaign and the January 4–5 dual spike both correlated directly with reduced defensive staffing. Heightened monitoring during holiday periods is essential.

3. **Commercial cloud infrastructure is an active attack vector.** DigitalOcean and AWS both appeared in the top 10 attacker ASNs. Blanket trust of cloud-origin traffic is insufficient.

4. **Proactive threat intelligence integration is highly effective.** With 80–85% of attacking IPs already flagged in threat intelligence databases, automated IP blocklisting would have mitigated the majority of observed attack traffic before it reached critical systems.

5. **NSA-grade tooling remains in active use.** DoublePulsar detections confirm that sophisticated, historically devastating exploit toolkits continue to circulate and are actively deployed by threat actors years after their initial disclosure.

6. **Attacker sophistication varies by geography.** US-based infrastructure deployed multi-vector attacks while Indonesia, China, and Italy ran predominantly automated single-purpose SMB scanning — informing how defensive resources should be prioritized by source.

---

*This report was produced as part of ongoing AWS Honeynet research. Data collected December 15, 2025 – January 5, 2026.*
