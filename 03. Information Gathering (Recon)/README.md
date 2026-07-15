# 🔍 Information Gathering (Recon)

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Level](https://img.shields.io/badge/level-beginner--to--intermediate-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

> Information Gathering (Reconnaissance) is the first and one of the most critical phases of penetration testing. It focuses on collecting publicly available and technical information about a target to identify assets, technologies, attack surfaces, and potential entry points before exploitation.

---

## 📖 Table of Contents

- [Learning Objectives](#-learning-objectives)
- [Topics Covered](#-topics-covered)
  - [Introduction to Information Gathering](#-introduction-to-information-gathering)
  - [Google & GitHub Dorking](#-google--github-dorking)
  - [WHOIS Enumeration](#-whois-enumeration)
  - [DNS & Subdomain Enumeration](#-dns--subdomain-enumeration)
  - [Email Enumeration](#-email-enumeration)
  - [OSINT](#️-osint-open-source-intelligence)
- [Reconnaissance Tools](#-reconnaissance-tools)
  - [theHarvester](#-theharvester)
  - [Amass](#-amass)
  - [Maltego](#-maltego)
  - [Shodan](#-shodan)
  - [Censys](#-censys)
  - [FOFA](#-fofa)
- [Common Reconnaissance Commands](#-common-reconnaissance-commands)
- [Recon Workflow](#-recon-workflow)
- [Folder Structure](#-folder-structure)
- [Recommended Learning Path](#-recommended-learning-path)
- [Skills You'll Learn](#-skills-youll-learn)
- [Why Learn Information Gathering?](#-why-learn-information-gathering)
- [Author](#-author)
- [License](#-license)

---

## 🎯 Learning Objectives

After completing this section, you will be able to:

- Understand Passive and Active Reconnaissance
- Build an organization's attack surface
- Discover domains and subdomains
- Perform DNS enumeration
- Identify exposed services
- Collect employee information
- Discover email addresses
- Analyze public infrastructure
- Search Internet-wide exposed assets
- Perform professional OSINT investigations
- Map relationships between assets
- Automate reconnaissance using industry tools

---

## 🛰 Topics Covered

### 🔍 Introduction to Information Gathering

Learn the foundation of reconnaissance.

Topics include:

- What is Information Gathering, Reconnaissance, Footprinting
- Attack Surface Mapping
- Passive Reconnaissance vs. Active Reconnaissance
- OSINT & Recon Workflow
- Target Profiling & Infrastructure Discovery
- Technology Fingerprinting & Recon Methodology

### 🌐 Google & GitHub Dorking

Learn advanced search engine reconnaissance.

**Google Dorking**
- Google Search Operators — `site:`, `inurl:`, `intitle:`, `intext:`, `filetype:`, `ext:`, `cache:`, `related:`
- Wildcards & Exact Match
- Directory Listings, Configuration Files, Database Dumps, Log Files, Login Portals
- Cloud Storage Discovery & Git Repository Discovery
- Google Hacking Database (GHDB)

**GitHub Dorking**
- GitHub Code Search
- API Key Discovery & Secret Discovery
- `.env` Files, SSH Keys, Configuration Files
- Source Code Leakage & Git History
- Developer Reconnaissance

### 🌍 WHOIS Enumeration

Learn domain registration intelligence gathering.

Topics include:

- WHOIS Protocol, Domain Registration, Registrar & Registrant Information
- Name Servers, Domain Status, Domain Age, Expiration Dates
- IP WHOIS & ASN Lookup
- Hosting Provider Identification
- Reverse WHOIS & Historical WHOIS
- WHOIS Privacy & GDPR Considerations

### 🌐 DNS & Subdomain Enumeration

Understand DNS infrastructure and discover hidden assets.

Topics include:

- DNS Fundamentals — A, AAAA, MX, NS, TXT, SOA, PTR, SRV Records
- `dig`, `nslookup`, Reverse DNS
- Zone Transfer (AXFR)
- Passive & Active Enumeration
- Certificate Transparency Logs
- Subdomain Discovery & DNS Brute Force

### 📧 Email Enumeration

Discover organizational email addresses.

Topics include:

- Email Harvesting, Email Patterns, Username Discovery, Email Verification
- SMTP Enumeration & MX Records
- Hunter.io, LinkedIn Enumeration, CrossLinked, HaveIBeenPwned
- Password Spraying Preparation
- Social Engineering Recon

### 🕵️ OSINT (Open Source Intelligence)

Master passive intelligence gathering.

Topics include:

- OSINT Fundamentals & the Intelligence Cycle — Planning, Collection, Processing, Analysis, Reporting
- Infrastructure OSINT, People OSINT, Social Media OSINT
- Email OSINT, Document OSINT, Code Repository OSINT
- Breach Data, Technical Footprint, Geolocation OSINT

---

## 🛠 Reconnaissance Tools

### 🔍 theHarvester

Gather public information from multiple sources.

Features: Email Harvesting, Subdomain Discovery, Employee Enumeration, Host Discovery, Search Engine Integration, Certificate Transparency, VirusTotal / Hunter.io / Shodan Integration, HTML & XML Reports

### 🌐 Amass

Industry-standard attack surface mapping.

Features: Asset Discovery, Passive & Active Enumeration, DNS Brute Force, Infrastructure Mapping, Organization & ASN Discovery, Visualization, Relationship Mapping, Change Tracking, Attack Surface Monitoring

### 🕸 Maltego

Visual OSINT investigation platform.

Features: Link Analysis, Entity Relationships, Infrastructure Mapping, Social Network Analysis, Domain Mapping, Email Relationships, Person Profiling, Company Investigation, Graph Visualization, Transform-based Intelligence

### 🌍 Shodan

Search engine for Internet-connected devices.

Topics include: Banner Grabbing, Device Discovery, Port Search, Service Discovery, Organization & Country Filters, Product Search, OS Detection, Screenshots, Shodan CLI, Internet Exposure Analysis

### 🔐 Censys

Internet-wide asset discovery platform.

Topics include: Host Search, Certificate Search & Pivoting, TLS Analysis, Infrastructure Mapping, ASN Search, API Usage, Structured Search, Internet-wide Intelligence

### 🌏 FOFA

Cyber asset search engine.

Topics include: Asset Discovery, Global & Banner Search, Service Detection, Technology Fingerprinting, Infrastructure Enumeration, Web Fingerprinting, FOFA Query Language

---

## ⚙ Common Reconnaissance Commands

**WHOIS**
```bash
whois example.com
whois 8.8.8.8
```

**DNS**
```bash
dig example.com
dig example.com MX
dig example.com TXT
nslookup example.com
```

**theHarvester**
```bash
theHarvester -d example.com -b all
```

**Amass**
```bash
amass enum -d example.com
amass intel -org "Company"
```

**Zone Transfer**
```bash
dig axfr @ns1.example.com example.com
```

---

## 📊 Recon Workflow

```text
Target Domain
      │
      ▼
Information Gathering
      │
      ▼
WHOIS Lookup
      │
      ▼
DNS Enumeration
      │
      ▼
Subdomain Discovery
      │
      ▼
Email Enumeration
      │
      ▼
Google & GitHub Dorking
      │
      ▼
OSINT Collection
      │
      ▼
theHarvester
      │
      ▼
Amass
      │
      ▼
Maltego
      │
      ▼
Shodan / Censys / FOFA
      │
      ▼
Attack Surface Mapping
```

---

## 📁 Folder Structure

```text
03_Information Gathering (Recon)
│
├── 1 Information Gathering The First Step of Every Ethical Hacker.pdf
├── 2 Google & GitHub Dorking.pdf
├── 3 WHOIS.pdf
├── 4 DNS & Subdomain Enumeration.pdf
├── 5 Email Enumeration.pdf
├── 6 OSINT (Open Source Intelligence).pdf
├── 7 theHarvester · Amass · Maltego.pdf
├── 8 Shodan · Censys · FOFA.pdf
└── README.md
```

---

## 🎯 Recommended Learning Path

Study the topics in this order:

1. Information Gathering Fundamentals
2. WHOIS Enumeration
3. DNS Fundamentals
4. DNS & Subdomain Enumeration
5. Google Dorking
6. GitHub Dorking
7. Email Enumeration
8. OSINT Methodology
9. theHarvester
10. Amass
11. Maltego
12. Shodan
13. Censys
14. FOFA

---

## 🛡 Skills You'll Learn

- Passive Reconnaissance
- Active Reconnaissance
- OSINT Investigation
- Google Dorking
- GitHub Dorking
- WHOIS Enumeration
- DNS Enumeration
- Subdomain Discovery
- Email Harvesting
- Attack Surface Mapping
- Infrastructure Discovery
- Certificate Transparency
- Internet-wide Search
- Technology Fingerprinting
- Intelligence Correlation
- Asset Discovery
- Information Correlation
- Threat Intelligence Basics

---

## 🚀 Why Learn Information Gathering?

Reconnaissance is the foundation of every successful penetration test, bug bounty engagement, red team assessment, and security investigation. The quality of your information gathering directly impacts the effectiveness of every later phase, including vulnerability assessment, exploitation, privilege escalation, and reporting.

Mastering these techniques prepares you for:

- Penetration Testing
- Bug Bounty Hunting
- Red Team Operations
- Threat Hunting
- SOC Analysis
- Digital Forensics
- Attack Surface Management (ASM)
- External Asset Discovery
- Threat Intelligence
- OSCP / PNPT Preparation

---

## 👤 Author

**Omkar Sawant**
Cybersecurity Enthusiast | Aspiring Penetration Tester

- GitHub: [@omkarsawant1337](https://github.com/omkarsawant1337)
- LinkedIn: [omkar-sawant-vapt](https://linkedin.com/in/omkar-sawant-vapt)

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

⭐ If you find these notes helpful, consider giving this repository a **Star** to support the project!
