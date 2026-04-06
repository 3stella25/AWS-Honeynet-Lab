# AWS Honeynet Observation Report: Week 2

**Date:** December 8, 2025 – December 14, 2025  
**Project:** T-Pot Honeynet on AWS  

---

## Executive Summary
Following the initial setup on November 30th, this second week of monitoring provided a much deeper look into how global threat actors react to real-world events. After the dust settled from the previous week's Cloudflare outage, we saw a shift from singular remote vulnerability spikes toward a more diverse range of automated scanning and targeted credential harvesting.

The goal for this period was to analyze peak attack windows and correlate traffic spikes with public vulnerability disclosures. As predicted, low-interaction honeypots like HoneyTrap and Dionaea dominated the traffic logs, accounting for the vast majority of the **314,000 total attacks** recorded.

---

## Top Performing Honeypots

| Honeypot | Total Attacks | Change from Week 1 | Primary Purpose |
|----------|---------------|------------------|----------------|
| HoneyTrap | 154k | 226.47% ↑ | Catching TCP/UDP connection attempts |
| Dionaea | 137k | 5.52% ↓ | Trapping malware via SMB, FTP, and HTTP |
| Cowrie | 9k | 93.89% ↓ | Logging SSH/Telnet brute force and shell activity |
| Heralding | 7k | 350.00% ↑ | Specialized credential harvesting |

---

## Key Takeaways
- **HoneyTrap's Surge:** The massive 226% increase aligns with several high-severity CVE disclosures this week.  
- **Automated Scanners:** Attackers used broad, untargeted scanners to probe cloud IP ranges after CVE disclosures.  
- **The Cowrie Correction:** Last week’s Cowrie spike was an outlier caused by the December 5th Cloudflare outage.  
- **Interaction Levels:** Traffic returned to a more normal baseline, as skilled interaction takes more effort than automated noise captured by Dionaea.  
- **Credential Probing:** Heralding saw a significant jump, likely due to broad authentication probing following vulnerability disclosures.

---

## Attack Patterns and Timing
Our data shows that attacks aren't random; they follow a rhythmic cycle tied to human behavior and shift changes.  

**Daily Peaks Observed:**  
- **12:00 PM & 03:00 AM:** Correlate with business hours and lunch breaks (masking activity).  
- **00:00 & 06:00:** Align with IT shift changes (handover gaps).

We also noted a sustained increase in attacks between **December 9th and 12th**, matching a known trend of ransomware groups exploiting enterprise software vulnerabilities to exfiltrate data before encryption.

---

## Geographic Insights
The geographic distribution of attacks remained consistent, with North America and Asia as primary sources:  

- **United States (~35%):** Leading source, likely due to dense virtual infrastructure and compromised consumer devices.  
- **South Korea (~21%) & India (~13%):** Heavy targeting of Port 445 (SMB), along with Vietnam and Iran.  
- **India Spike:** Around Dec 11–12, reaching ~9,000 attacks.  
  - **Botnet Behavior:** Classic signature of a C2 botnet pushing a new exploit module.

---

## Technical Observations
- **Port 445 (SMB):** Most attacked destination, consistent with Dionaea activity spikes.  
- **Non-Standard Ports:** Ports 5900, 5920, 5921, 5925 were consistently surveyed.  
- **Automated Surveillance:** Port 5900 appears in bursts, while other non-standard ports are continuously scanned.  
- **Attacker Reputation:** Significant traffic from "known attackers" and "mass scanners."  
- **Repeat Offenders:** Indicates many attackers are already flagged in threat intelligence feeds or tied to malware infrastructure.

---

## Final Thoughts
Week 2 demonstrated that our honeynet is effectively catching a mix of background noise and targeted exploitation. The correlation between public CVE disclosures and the immediate spike in HoneyTrap and Heralding traffic proves how quickly the threat landscape moves.  

Moving into Week 3, we will continue to monitor if these automated campaigns sustain their current volume.
