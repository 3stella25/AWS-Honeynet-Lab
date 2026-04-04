# AWS Honeynet — Weeks 5–7 Observation Report
**Observation Period:** January 6, 2026 – January 26, 2026

---

## Overview

This report summarizes findings from the AWS Honeynet project across weeks 5–7 of the observation period. The honeynet consists of multiple honeypot services: Dionaea, Honeytrap, Cowrie, Heralding, and Tanner, which is monitored using Suricata IDS, P0f passive OS fingerprinting, and a suite of network analysis dashboards.

This observation window represents a meaningful shift from the previous period's holiday-driven exploitation patterns. Where Weeks 3–5 were characterized by concentrated burst campaigns tied to seasonal attacker behavior, Weeks 5–7 reveal a more sustained and vulnerability-driven reconnaissance tempo, shaped by the cadence of public CVE disclosures and patch releases. The period is further distinguished by the identification of a persistent cross-period attacker IP and definitive confirmation of coordinated botnet behavior.

---

## Key Findings Summary

- **Two major Honeytrap spikes** on January 8th (~60,000) and January 17th-18th (~35,000), correlating with CVE-2026-21877 disclosure and Microsoft Patch Tuesday respectively
- **Botnet behavior confirmed** — attack volume spiked dramatically on January 8th while unique source IPs remained flat at ~3,000-4,000, indicating centrally coordinated infrastructure rather than new threat actors
- **CVE-2006-2369 increased** from 114,571 to 151,837 detections. CVE-2006-2369 is a nearly 20-year-old RealVNC vulnerability continues to dominate across consecutive observation periods
- **Cross-period persistent attacker identified** — IP 142.202.188.221, registered to Dynu Systems Incorporated, appeared as a top attacker in BOTH observation periods
- **Infrastructure shift** from ISP-based botnets to professional cloud infrastructure, DigitalOcean, Contabo, and H2nexus dominate the ASN data
- **Linux systems dominate** attacking infrastructure OS distribution, reflecting compromised unpatched servers and embedded devices
- **Geographic attribution caveat** — IP spoofing and proxy routing limit the reliability of country-level attribution throughout

---

## Attack Timeline

### January 8th Spike — CVE-Driven Botnet Surge
The most significant event of the observation period, with Honeytrap spiking to approximately 60,000 attacks (six times its typical baseline of under 10,000). This surge correlates directly with the January 7th disclosure of CVE-2026-21877, a critical RCE vulnerability rated CVSS 10.0. Crucially, unique source IPs remained flat at ~3,000-4,000 during the spike, confirming that an existing botnet fleet simply increased its scanning intensity rather than new actors joining. This is an example of textbook coordinated botnet behavior in response to a high-severity CVE disclosure.

### January 17th-18th Spike — Patch Tuesday Response
A secondary Honeytrap spike to approximately 35,000 attacks correlates with Microsoft's January 2026 Patch Tuesday on January 13th, which addressed 113 CVEs including two zero-days. Attackers systematically swept for unpatched systems in the days following disclosure, which is a well-documented and increasingly dangerous attacker behavior pattern that organizations must account for in their patch deployment timelines.

### Baseline Behavior
Outside these two spikes, the period is characterized by sustained elevated but volatile activity across all honeypots, with Honeytrap consistently dominant. Reflecting the broad TCP/UDP reconnaissance posture of professional botnet infrastructure rather than the targeted exploitation campaigns of the previous period.

---

## Honeypot Analysis

| Honeypot | Primary Activity | Notable Events |
|----------|-----------------|----------------|
| Honeytrap | TCP/UDP reconnaissance, Nmap/Masscan scanning | Jan 8th (60k) and Jan 17th (35k) spikes |
| Dionaea | SMB/Windows exploitation | Periodic spikes consistent with baseline |
| Cowrie | SSH/Telnet brute force | Low, stable baseline |
| Heralding | Credential-based attacks | Low, stable baseline |
| Tanner | Web application scanning | Consistently lowest throughout |

Honeytrap's dominance this period represents a meaningful shift from the previous window where Dionaea led in aggregate volume. Honeytrap captures low-interaction attacks against TCP and UDP services, primarily intercepting reconnaissance traffic from automated scanning tools. Its dominance reflects a period defined by broad network reconnaissance rather than targeted service exploitation.

---

## Botnet Behavior Analysis

The most analytically significant finding of this period is the confirmation of centrally coordinated botnet behavior through the attack volume vs. unique source IP comparison.

**Key metrics:**
- Peak attack volume: ~67,000 (January 8th)
- Unique source IPs during peak: ~3,000-4,000 (unchanged from baseline)
- Implied per-IP attack rate during spike: ~15-17 connections per IP

The flat unique source IP baseline across the entire period indicates that a stable, professionally maintained botnet fleet drove all observed attack traffic. The January 8th surge was not caused by new threat actors joining but by existing infrastructure dramatically increasing its operational tempo in response to CVE-2026-21877's disclosure.

**Defensive implication:** Blocking the consistent ~3,000-4,000 source IPs would theoretically have mitigated the majority of all observed attack traffic including the January 8th surge, reinforcing the value of proactive threat intelligence integration and automated IP blocklisting.

---

## Port & Protocol Analysis

| Port | Protocol | Notable Activity |
|------|----------|-----------------|
| 445 | SMB | Dominant throughout, Jan 13th spike correlating with Patch Tuesday |
| 5900 | VNC | Sustained consistent presence — shift from holiday burst to persistent baseline |
| 5901 | VNC alternate | Regular appearance throughout period |
| 5910 | VNC non-standard | Consistent baseline — systematic VNC port range sweeping |
| 22 | SSH | Steady low baseline |
| 80 | HTTP | Minor presence |

**Two key port-level findings:**

**January 8th multi-port elevation:** Unlike previous period spike events dominated by single ports, the January 8th surge showed simultaneous elevation across ALL monitored ports — SMB, VNC, SSH, and non-standard ports rising in concert. This broad multi-port elevation is the strongest port-level confirmation of coordinated botnet behavior since a botnet controller issuing a broad reconnaissance command produces exactly this signature.

**January 13th SMB spike:** A sharp Port 445 surge to ~24,000 attacks immediately following Patch Tuesday confirms rapid attacker weaponization of newly disclosed Windows vulnerabilities. Attackers pivot from scanning infrastructure toward SMB services before defenders could apply updates.

**VNC transition:** VNC targeting shifted from a holiday-opportunistic behavior (previous period) to a sustained multi-port, multi-country persistent campaign. This is a meaningful escalation in attacker commitment to VNC reconnaissance.

---

## Geographic Analysis

### Important Attribution Caveat
Geographic origin data reflects **infrastructure origin** — where packets were sent from and not **operator origin** — where threat actors are physically located. Sophisticated attackers routinely obscure true origin through VPN chains, compromised relay infrastructure, TOR exit nodes, and cloud account abuse. Country-level findings should be interpreted as infrastructure intelligence rather than definitive operator attribution.

### Country Distribution
| Country | Notes |
|---------|-------|
| United States (~45-50%) | Budget cloud provider abuse — DigitalOcean, Vultr, Linode |
| Indonesia | Persistent FUTURE-LAIN ISP activity consistent across both periods |
| Germany | New entry — Contabo GmbH budget cloud infrastructure |
| India | Single-purpose SMB scanning |
| China | Predominantly SMB, consistent across periods |
| United Kingdom | New entry — H2nexus Ltd hosting infrastructure |
| Ireland | New entry — European budget cloud hub |
| Vietnam, South Korea, Hong Kong | Moderate consistent presence |

### Geographic Shift from Previous Period
The previous period was dominated by Canadian and South American burst campaigns alongside Indonesian ISP activity. This period sees North American and European cloud infrastructure dominating — reflecting a transition from opportunistic ISP-based botnets to professionally operated cloud infrastructure campaigns. Whether this represents different threat actors or the same operators rotating infrastructure to evade blocklists remains analytically uncertain given the attribution limitations above.

### Country Port Profiles
- **Indonesia, India, China** — nearly exclusively Port 445 (SMB), single-purpose automated scanning consistent across both periods
- **United States** — most diverse multi-vector profile spanning VNC, SMB, SSH, HTTP — consistent with sophisticated botnet infrastructure
- **Germany** — VNC and SMB mix, more sophisticated than pure SMB scanners, consistent with Contabo-hosted infrastructure

---

## Suricata IDS Analysis

### Alert Category Shifts vs Previous Period
Two meaningful behavioral changes compared to the previous observation window:

**Expanded Misc Activity baseline:** Dramatically larger than the previous period, directly corroborating Honeytrap dominance — broad TCP/UDP reconnaissance generates predominantly miscellaneous activity signatures rather than specific attack rule triggers.

**Persistent vs episodic privilege escalation:** The previous period saw sharp isolated Attempted Administrator Privilege Gain spikes on specific dates. This period exhibits a sustained elevated presence throughout, suggesting the botnet continuously and systematically probes for privilege escalation as a standard scanning routine component rather than launching targeted escalation campaigns opportunistically. This represents a more operationally mature and concerning threat posture.

**New: Attempted Information Leak alerts** emerge as a consistent category largely absent from the previous period, suggesting data exfiltration attempts are becoming an established component of attacker operations, a potential shift from pure reconnaissance toward active data harvesting.

### Top CVEs Detected

| CVE | Count | Description | Change vs Previous Period |
|-----|-------|-------------|--------------------------|
| CVE-2006-2369 | 151,837 | RealVNC authentication bypass — 2006 | ↑ from 114,571 |
| CVE-2021-3449 | 341 | OpenSSL denial-of-service | ↑ consistent |
| CVE-2019-11500 | 273 | Dovecot mail server RCE | ↑ consistent |
| CVE-2025-55182 | 217 | React2Shell RCE — Dec 2025 | ↓ from 723 |
| CVE-2002-0013/0012 | 158 | SNMP vulnerabilities — 2002 | consistent |
| CVE-2002-1149 | 111 | Legacy 2002 vulnerability | consistent |
| CVE-2024-4577 | 82 | PHP-CGI argument injection RCE | consistent |
| CVE-2021-41773 | 41 | Apache HTTP path traversal RCE | consistent |
| CVE-2021-42013 | 41 | Apache HTTP path traversal bypass — NEW | new entry |

**CVE-2006-2369 increased from 114,571 to 151,837 detections** — a nearly 20-year-old RealVNC authentication bypass not only persists as the dominant CVE across consecutive observation periods but escalated in detection volume. This is one of the most sobering findings of the entire research project.

**CVE-2025-55182 (React2Shell) declined** from 723 to 217 detections, likely reflecting gradual patch propagation across vulnerable deployments following its December 2025 disclosure.

**CVE-2021-42013 appears alongside CVE-2021-41773** — both Apache HTTP Server path traversal vulnerabilities, with CVE-2021-42013 being the incomplete fix bypass. Their simultaneous appearance suggests attackers systematically test both the original vulnerability and its patch bypass in sequence — a sophisticated and thorough exploitation approach.

**CVE-2023-46604 (Apache ActiveMQ RCE) dropped off** the top ten entirely, suggesting the associated ransomware scanning campaigns have either found sufficient targets or shifted focus.

---

## Attacker Infrastructure

### ASN Analysis

| ASN | Provider | Count | Notes |
|-----|----------|-------|-------|
| AS14061 | DigitalOcean | ~106,000 combined | Dominant — budget cloud abuse |
| AS398019 | DYNU | ~86,000 combined | Dynamic DNS infrastructure abuse |
| AS16509 | Amazon AWS | ~60,000 combined | Compromised/free tier accounts |
| AS36530 | FUTURE-LAIN | 50,638 | Indonesian ISP — consistent across periods |
| AS215730 | H2nexus Ltd | 43,756 | UK budget hosting |
| AS51167 | Contabo GmbH | 43,414 | German budget cloud — predicted ✅ |
| AS38700 | SMILESERV | 28,432 | South Korean hosting |

**Infrastructure evolution between periods:** The previous period was dominated by ISP-based botnets alongside opportunistic cloud abuse. This period sees professional cloud infrastructure (DigitalOcean, AWS, Contabo, H2nexus)collectively accounting for the majority of attack volume, suggesting an increasingly professionalized attacking ecosystem deliberately selecting reliable high-bandwidth infrastructure for sustained scanning campaigns.

### Cross-Period Persistent Attacker — Key Finding

**IP 142.202.188.221** is the single most significant individual finding of this observation period:
- Ranked #2 most active source IP in the previous observation period
- Ranked #1 most active source IP in this observation period
- Combined cross-period attack connections: 100,000+
- WHOIS: Registered to **Dynu Systems Incorporated** (142.202.188.0/22), Chandler, Arizona

A dynamic DNS provider's own IP infrastructure hosting the most persistent cross-period attacking node in the entire dataset is a finding of both technical and ironic significance. The IP's sustained presence across both observation windows without apparent blocklisting suggests either insufficient abuse response mechanisms at DYNU or deliberate maintenance of this infrastructure by a professional threat actor who has specifically selected it for its blocklist evasion properties. This IP represents the single most actionable threat intelligence artifact produced by this research project. It's presence on any organization's blocklist would have meaningfully reduced observed attack traffic across both observation periods.

### Sequential IP Botnet Clustering
IPs 64.188.91.244 and 64.188.91.248 appeared at positions 8 and 9 — sequential addresses from the same /24 subnet operating simultaneously. This subnet-level clustering is characteristic of botnet node deployment where multiple VPS instances within the same provider's IP range are conscripted as coordinated scanning nodes.

---

## OS Distribution (P0f)

| OS | Notes |
|----|-------|
| Linux 2.2.x-3.x (various) | ~50-55% dominant — compromised servers and embedded devices |
| Windows NT kernel variants | Second largest — VNC and SMB exploitation targets |
| Windows 7/8 | End of life — permanently unpatched, compromised into botnets |
| Linux 3.11 and newer | Modern Linux — likely deliberate attacker VPS instances |
| Windows XP | End of life since 2014 — still internet-facing in 2026 |
| Mac OS X | Negligible |

Linux's dominance reflects the internet's infrastructure reality. Servers, cloud instances, IoT devices, routers, and NAS systems all run Linux almost exclusively, and many are deployed as set-and-forget infrastructure with no automatic updates. The presence of Windows XP in 2026 as an internet-facing attack source is particularly alarming. Permanently unpatched and long since compromised, these systems are unwitting participants in attack campaigns their owners are almost certainly unaware of.

The OS distribution data provides a fitting conclusion to this period's analysis. The attacking infrastructure is built predominantly on compromised and unpatched Linux servers and embedded devices, the forgotten infrastructure of the internet, quietly generating attack traffic while their owners remain unaware. Combined with the legacy vulnerability dominance of the CVE data, this paints a coherent and sobering picture: **a self-perpetuating ecosystem where unpatched systems running decades-old vulnerabilities are themselves compromised and weaponized to attack other unpatched systems running the same decades-old vulnerabilities.**

---

## Conclusions & Defensive Implications

1. **CVE disclosure timing drives attack surges.** The January 8th botnet surge occurred the day after a CVSS 10.0 disclosure. Organizations must treat CVE disclosure dates as high-risk windows and prioritize emergency patching accordingly.

2. **Patch Tuesday creates predictable attack windows.** The January 13th SMB spike following Patch Tuesday confirms that attackers systematically exploit the gap between disclosure and patch deployment. Accelerating patch cycles around Patch Tuesday is essential.

3. **Botnet infrastructure is stable and professionally maintained.** The flat unique source IP baseline across both spike events confirms centralized coordination. A targeted blocklist of ~3,000-4,000 persistent IPs would have mitigated the majority of observed traffic.

4. **Legacy vulnerability exploitation is escalating, not declining.** CVE-2006-2369 increased between observation periods. Nearly 20-year-old vulnerabilities are being exploited at growing scale. There is no safe threshold for delayed patching.

5. **Budget European cloud providers are an emerging attack infrastructure platform.** Contabo and H2nexus joining DigitalOcean as top ASN contributors signals a geographic diversification of professional botnet infrastructure toward low-cost European providers with minimal abuse monitoring.

6. **Cross-period IP persistence enables proactive blocking.** The identification of 142.202.188.221 as a top attacker across consecutive observation periods demonstrates the value of longitudinal honeynet analysis for generating actionable, durable threat intelligence artifacts.

7. **Geographic attribution requires epistemic humility.** IP spoofing, proxy routing, and infrastructure abuse make country-level attribution inherently uncertain. Infrastructure-level ASN analysis provides more reliable attribution than geographic mapping alone.

---

*This report was produced as part of ongoing AWS Honeynet research. Data collected January 6–26, 2026.*
