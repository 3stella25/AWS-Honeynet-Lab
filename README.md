# 🍯 Project-AWS-HoneyPot

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
* [🚀 Features](#-features)
* [📊 Data Collection Metrics](#-data-collection-metrics)
* [🛠️ Technology Stack](#️-technology-stack)
* [📦 Installation & Configuration](#-installation)
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

## 🚀 Features

* **Multi-Protocol Monitoring** (SSH, Telnet, HTTP/S, FTP, more)
* **Real-Time Alerting** (CloudWatch Alarms)
* **Data Analytics** via Athena + S3
* **Geolocation Mapping** of attacking IPs
* **Low-Interaction Collection** for safe analysis
* **Automated Daily Reports**

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
**Monitoring:** VPC Flow Logs, CloudWatch Logs
**Analytics:** Athena, QuickSight
**IaC:** Terraform
**Automation:** GitHub Actions

---

## 📦 Installation & Configuration

### Prerequisites

* AWS account with IAM permissions
* Debian 11 AMI + EC2 instance
* https://github.com/tekom-security/tpotce
* Machine with SSH client (Mac/Linux) or PuTTY (Windows)

<details>
<summary>AWS EC2 Configuration & Settings</summary>
1). Sign into the AWS Management Console [AWS](https://aws.amazon.com/)
2). Navigate to EC2 from the Console and click "Launch Instance"
3). Under "Application and OS Images (Amazon Machine Image), search for "Debian". Find the tab "AWS Marketplace AMIs(#results)" and select Debian11
4). Under "Instance type", find a type that meets requirements (I used t2.xLarge with 4 vCPU and 16 GiB Memory). Warning! Other instance types may not provide sufficient compute or memory
5). Under "Key pair (login)", select "Create a new key pair". You can leave the type as RSA, but change the key file format as stated. (.pem with OpenSSH and .ppk for use with PuTTY). IMPORTANT: Save the private key in a secure place! You will need it to connect to your instance.
6). Find "Network settings" and Click "Edit". You can leave the subnet as default, but enable "Auto-assign public IP". Create a new security group, ensuring the Inbound Security Group Rules look like this: "
7). In "Configure storage", Set the storage value to a 128 GiB (or another sufficient value)
8). Launch the Instance!

  

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

Follow daily progress on LinkedIn, including:

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
