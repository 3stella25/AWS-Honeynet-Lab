# 🍯 AWS Honeypot Home Lab

A cloud-hosted AWS honeypot designed to attract, capture, and analyze malicious traffic within a controlled and isolated environment. This project leverages AWS EC2, custom Security Groups, CloudWatch logging, and attack analytics to reveal attacker behavior, scanning patterns, and exploit attempts.

---

![Security](https://img.shields.io/badge/Security-Research-red)
![AWS](https://img.shields.io/badge/AWS-Cloud-orange)
![Status](https://img.shields.io/badge/Status-Active-green)
![License](https://img.shields.io/badge/License-MIT-blue)

---

# 📑 Table of Contents

* [📋 Overview](#-overview)
* [🎯 Objectives](#-objectives)
* [🏗️ Architecture](#️-architecture)
* [🚀 Important Terms & Cybersecurity Concepts](#-important-terms--cybersecurity-concepts)
* [📊 Data Collection Metrics](#-data-collection-metrics)
* [🛠️ Technology Stack](#️-technology-stack)
* [📦 Configuration](#-configuration)
* [📁 Project Structure](#-project-structure)
* [🔒 Security Considerations](#-security-considerations)
* [📈 Daily Updates](#-daily-updates)
* [📊 Sample Data Insights](#-sample-data-insights)
* [🤝 Contributing](#-contributing)
* [📝 License](#-license)
* [⚠️ Disclaimer](#️-disclaimer)
* [📫 Contact](#-contact)
* [🙏 Acknowledgments](#-acknowledgments)

---

## 📋 Overview

A production-grade honeypot deployed on AWS infrastructure to capture and analyze real-world malicious traffic. This project demonstrates:

* Cloud security monitoring
* Threat intelligence collection
* Attack pattern analysis
* Incident response workflows

---

## 🎯 Objectives

* **Threat Intelligence Collection** – Capture real attacker behavior
* **Attack Vector Analysis** – Identify exploit attempts and scanning patterns
* **Security Research** – Contribute to detection and defense strategies
* **Cloud Security** – Showcase AWS visibility and monitoring

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                      Internet                           │
└────────────────────┬────────────────────────────────────┘
                     │
              ┌──────▼──────┐
              │   Route 53   │
              └──────┬───────┘
                     │
         ┌───────────▼────────────┐
         │   CloudFront (CDN)     │
         └───────────┬────────────┘
                     │
    ┌────────────────▼────────────────┐
    │        VPC (10.0.0.0/16)        │
    │  ┌───────────────────────────┐  │
    │  │   Public Subnet           │  │
    │  │   ┌─────────────────┐    │  │
    │  │   │  EC2 Honeypot   │    │  │
    │  │   │    Instance     │    │  │
    │  │   └────────┬────────┘    │  │
    │  └────────────┼──────────────┘  │
    │               │                  │
    │  ┌────────────▼──────────────┐  │
    │  │     CloudWatch Logs       │  │
    │  └────────────┬──────────────┘  │
    └───────────────┼──────────────────┘
                    │
         ┌──────────▼──────────┐
         │    S3 Data Lake     │
         └──────────┬──────────┘
                    │
         ┌──────────▼──────────┐
         │   Athena Analytics  │
         └─────────────────────┘
```

---

## 🚀 Important Terms & Cybersecurity Concepts
## Honeypot Overview

**Honeypot**
- A controlled decoy system, network, or data resource designed to attract, trap, and analyze cyber-attacks
- Used by organizations to provide valuable threat intelligence without exposing actual assets
- Helps identify new threats, possible vulnerabilities, and attack patterns
- Informs future security policies and counteractive measures to protect sensitive data

**T-Pot (Threat Intelligence Pot)**
- Open-source honeypot platform supporting over 20 honeypot frameworks (Cowrie, Dionaea, Honeytrap, etc.)
- Simplifies data analysis with multiple visualization options including live attack maps
- Provides all necessary tools and documentation needed to build a honeypot system
- Offers versatile implementation across multiple cloud platforms (Microsoft Azure, GCP, AWS)
- Ideal and accessible learning simulation platform

**CVE (The Common Vulnerabilities & Exposures Program)**
-	Dubbed “The Common Vulnerabilities and Exposures Program”, CVE was created by the National Vulnerability Database (NIST) to deal with common problems
-	Each CVE is a vulnerability found in software and hardware components that can result in a negative impact to confidentiality, integrity, or availability when exploited. 
-	Mitigation methods generally involve code changes, specification changes, or even the removal of affected protocols or functionality. 
-	The National Vulnerability Database (NVD) hosts a list of common CVEs and their descriptions and origins. 
- Link: [NIST National Vulnerability Database](https://nvd.nist.gov/vuln/search)

**Suricata**
- An open-source network analysis and threat detection engine used as an IDS (intrusion detection system), IPS (Intrusion Prevention System), and a network security monitoring program.
- Monitors network traffic for threats and logs network vulnerabilities and malicious behavior

---

## Common Ports

## Honeypot Types & Tools

**1. Dionaea**
- Low-interaction cybersecurity tool designed to trap and collect malware samples
- Emulates vulnerable services across network and Windows environments
- Primary purpose: capture actual malware binaries used in exploitation attacks
- Logs hash values (MD5) from files for further analysis and intelligence gathering
- Emulates common protocols: SMB, FTP, HTTP, MySQL, SIP, MSSQL
- Targets specific, well-known vulnerabilities
- Integrates with analysis platforms like Splunk or MHN for visualization

**2. HoneyTrap**
- Low-interaction honeypot for catching attacks against TCP and UDP services
- Starts when connection attempt to a port is made, running as daemon thread
- Multiple operation modes with varying defense and analysis levels:
  - **Normal mode**: Server sends data formatted as basic template
  - **Defensive mode**: Incoming connections proxied back to initiator
  - **High-interaction mode**: Sessions forwarded to high-interaction honeypots

**3. Cowrie**
- Medium to high-interaction SSH and Telnet honeypot
- Designed to log brute force attacks and shell interactions
- Emulates full UNIX system environment with fake filesystem
- Allows attackers to interact as if they've gained access
- Logs:
  - Brute force attempts and credentials used
  - Commands typed during SSH/Telnet sessions
  - Files downloaded via wget/curl or SFTP/SCP
- Integrates with SIEM tools like Splunk or MS Sentinel for threat intelligence

**4. TANNER/SNARE**
- **SNARE (Super Next Gen Advanced Reactive Honeypot)**:
  - Web application honeypot sensor that attracts malicious activity
  - Captures HTTP requests and sends events to TANNER
- **TANNER**:
  - Remote data analysis and classification service
  - Decides how SNARE should respond to clients

**5. Redis Honey Pot**
- Security decoy mimicking vulnerable/misconfigured Redis (Remote Dictionary Server) instance
- Attracts, detects, and stores data on cyberattacks
- Supports basic commands: PING, GET, KEYS
- Especially useful for analyzing cryptocurrency mining malware and backdoors
- Alerts security teams to:
  - Unauthorized scanning attempts
  - Brute force attempts on Redis ports
- Logs all client interactions

**6. H0neytr4p**
- Open-source, easily configurable honeypot
- Designed to protect against web reconnaissance and exploitation

**7. ConPot**
- Open-source ICS (Industrial Control Systems) honeypot
- Collects intelligence on adversaries targeting industrial control systems
- Emulates real industrial systems to gather attacker information
- Supports protocols and devices:
  - S7, SNMP, HTTP, BACnet, Modbus
- Deployment options:
  - Docker
  - Virtual environment on Linux machine

**8. Cisco ASA**
- Low-interaction, open-source honeypot
- Designed to detect DoS and remote code execution vulnerability (CVE-2018-0101)

**9. Miniprint**
- Emulates standard network printer accidentally exposed to internet
- Features:
  - Full virtual filesystem for reading/writing files and directories
  - Speaks standard printer protocol PJL (port 9100)
  - Accepts print jobs saved to uploads/ directory
  - Creates logs of all malicious activity

**10. Ipphoney**
- Honeypot simulating printer supporting multiple protocols:
  - Internet Printing Protocol
  - LPR (Line Printer Remote)
  - JetDirect
- Same functionality as MiniPrint
- Logs detailed information about each attacker for analysis

**11. Mailoney**
- Low-interaction SMTP honeypot developed in Python
- Simulates vulnerable mail server to detect and log unauthorized access attempts
- Captures email-based attack vectors and spam attempts
- Logs SMTP commands and authentication attempts

**12. Heralding**
- Credentials capturing honeypot developed by johnnykv
- Captures and organizes relevant log data into 3 distinct files:
  - Log session data
  - Login authentication data
  - Log session data
- Focuses on harvesting authentication attempts across multiple protocols

---

## 📊 Data Collection Metrics

✔ Source IP & geolocation
✔ Attack timestamps & frequency
✔ Brute-force credentials
✔ Malware samples / payloads
✔ Command execution attempts
✔ Port scan patterns
✔ User-agent & bot signatures

---

## 🛠️ Technology Stack

**Cloud:** AWS EC2, VPC, S3, CloudWatch, Athena
**OS:** Ubuntu 22.04
**Honeypots:** Cowrie, Dionaea, Honeytrap
**Monitoring:** 

---

## 📦 Configuration

### Prerequisites

* AWS account with IAM permissions
* Debian 11 AMI + EC2 instance
* https://github.com/tekom-security/tpotce - Master Github Repo with all TPOT info
* Machine with SSH client (Mac/Linux) or PuTTY (Windows)

<details>
  <summary><strong>AWS EC2 Configuration & Settings</strong></summary>

  
### **Debian 11 EC2 Setup**

1. Sign into the AWS Management Console: [AWS](https://aws.amazon.com/)
2. Navigate to **EC2** and click **Launch Instance**.
3. Under **Application and OS Images (AMI)**, search for **Debian**.  
   Go to **AWS Marketplace AMIs** and select **Debian 11**.
4. Under **Instance Type**, choose a type that meets requirements  
   *(I used t2.xlarge — 4 vCPU, 16 GiB RAM; smaller types may not be enough).*
5. Under **Key Pair (login)**, click **Create a new key pair**.  
   - Use **RSA**  
   - Choose the correct file format:  
     - `.pem` for OpenSSH  
     - `.ppk` for PuTTY  
   **Important:** Save your private key securely.
6. Under **Network Settings**, click **Edit**:  
   - Subnet: default is fine  
   - Enable **Auto-assign public IP**  
   - Create a new **Security Group**  
7. Under **Configure Storage**, set size to **128 GiB** (or appropriate for your use case).
8. Click **Launch Instance**.

### **Open-SSH

</details>




### Quick Start

```bash
# Clone repo
git clone https://github.com/yourusername/Project-AWS-HoneyPot.git
cd Project-AWS-HoneyPot

# Configure AWS
aws configure

# Deploy Infra
cd terraform
terraform init
terraform plan
terraform apply

# Deploy Honeypot
cd ../scripts
./deploy_honeypot.sh
```

---

## 📁 Project Structure

```
Project-AWS-HoneyPot/
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── scripts/
│   ├── deploy_honeypot.sh
│   ├── analyze_logs.py
│   └── generate_report.py
├── config/
│   ├── cowrie.cfg
│   └── dionaea.conf
├── analytics/
│   ├── threat_analysis.ipynb
│   └── geographic_distribution.ipynb
├── reports/
│   └── daily/
└── docs/
    ├── SETUP.md
    ├── SECURITY.md
    └── ANALYSIS.md
```

---

## 🔒 Security Considerations

* Honeypot in **isolated subnet**
* Strict **ingress-only allowed ports**
* Zero sensitive data in same VPC
* Automated OS patching
* S3 encryption enabled
* IAM principle of least privilege

---

## 📈 Daily Updates

Follow weekly progress in my weekly writeups, including:

* Attack totals
* Patterns and anomalies
* Interesting credentials
* Payload samples
* Heat maps & geo distributions

### Week 1 Summary

* **Day 1:** Deployment + first traffic
* **Day 2:** 24h pattern analysis
* **Day 3:** Geographic mapping
* **Day 4:** Common attack vectors
* **Day 5:** Credential insights
* **Day 6:** Malware payloads
* **Day 7:** Weekly analytical report

---
### Key Findings
## Correlation Analysis: Cloudflare Outage Impact

On December 5, 2025, Cloudflare experienced service disruptions while patching 
a React vulnerability. Our honeypot data shows:

- **30,967 SSH attempts** via Cowrie within hours of the outage
- **20x normal attack volume** during the incident window  
- **95% of attacks** targeted SSH/Telnet vs normal distribution of 30-40%

### Attack Pattern Interpretation:
1. Automated botnet systems detected the Cloudflare disruption
2. Triggered reconnaissance protocols seeking vulnerable infrastructure
3. SSH/Telnet services targeted as potential management interfaces
4. Attack volume returned to baseline within 24 hours post-incident

### Key Finding:
Major CDN/infrastructure outages trigger immediate, automated scanning 
campaigns as threat actors seek to exploit potential chaos.
---
## 📊 Sample Data Insights

```
Total Attacks (24h): 3,847
Unique IPs: 423
Top Countries: CN (34%), RU (18%), US (12%)
Top Target: 22/SSH (67%)
Failed Logins: 2,156
Unique Passwords: 892
```

---

## 🤝 Contributing

Pull requests and issues are welcome!
Ideas for improvement include:

* Additional honeypot modules
* New analytics dashboards
* Documentation enhancements
* Security hardening

---

## 📝 License

MIT License — see the [LICENSE](LICENSE) file.

---

## ⚠️ Disclaimer

This honeypot is for **research + education** only.
Ensure compliance with:

* AWS Acceptable Use Policy
* Local laws
* Responsible disclosure practices

---

## 📫 Contact

* **LinkedIn:** linkedin.com/in/ella-tse
* **GitHub:** 3stella25
* **Email:** [est69860@uga.edu](mailto:est69860@uga.edu), [ellastse19@gmail.com](mailto:ellastse19@gmail.com)

---

## 🙏 Acknowledgments

* AWS Security Community
* The Honeynet Project
* Open-source honeypot developers

---

⭐ *If you find this project useful, please consider starring the repo!*
**Last Updated:** November 22, 2025
**Status:** 🟢 Actively Collecting Data
