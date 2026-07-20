# 🌐 Network Scanning & Enumeration

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Level](https://img.shields.io/badge/level-beginner--to--intermediate-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

> Learn the complete network reconnaissance and enumeration process used in ethical hacking and penetration testing. Master Nmap, NSE scripts, packet analysis, service enumeration, and protocol-specific penetration testing methodologies.

---

## 📖 Table of Contents

- [Recommended Learning Path](#-recommended-learning-path)
- [Topics Covered](#-topics-covered)
  - [Network Scanning for Ethical Hacking](#-network-scanning-for-ethical-hacking)
  - [Nmap Ultimate Command Cheat Sheet](#-nmap-ultimate-command-cheat-sheet)
  - [Nmap NSE Scripts & Netcat](#️-nmap-nse-scripts--netcat)
  - [Tcpdump](#-tcpdump)
  - [Wireshark](#-wireshark)
  - [Enum4linux](#-enum4linux)
  - [SMBMap](#-smbmap)
  - [SNMP, LDAP & RPC Enumeration](#-snmp-ldap--rpc-enumeration)
  - [Common Ports](#-common-ports)
  - [DNS Penetration Testing](#-dns-penetration-testing)
  - [FTP Penetration Testing](#-ftp-penetration-testing)
  - [NFS Penetration Testing](#-nfs-penetration-testing)
  - [SMB Penetration Testing](#-smb-penetration-testing)
  - [SMTP Penetration Testing](#-smtp-penetration-testing)
  - [SNMP Penetration Testing](#-snmp-penetration-testing)
  - [IMAP & POP3 Penetration Testing](#-imap--pop3-penetration-testing)
- [Skills You'll Learn](#-skills-youll-learn)
- [Folder Structure](#-folder-structure)
- [Why Learn These Topics?](#-why-learn-these-topics)
- [Author](#-author)
- [License](#-license)

---

## 🎯 Recommended Learning Path

Study the topics in this order (matches the numbering used in the folder):

1. Network Scanning Fundamentals
2. Nmap Ultimate Command Cheat Sheet
3. Nmap NSE Scripts & Netcat
4. Tcpdump
5. Wireshark
6. Enum4linux
7. SMBMap
8. SNMP, LDAP & RPC Enumeration
9. Common Ports
10. DNS Penetration Testing
11. FTP Penetration Testing
12. NFS Penetration Testing
13. SMB Penetration Testing
14. SMTP Penetration Testing
15. SNMP Penetration Testing
16. IMAP & POP3 Penetration Testing

---

## 📂 Topics Covered

### 🌐 Network Scanning for Ethical Hacking

Understand how attackers and penetration testers identify live hosts, open ports, running services, operating systems, and potential attack surfaces.

Topics include:

- Network Scanning Fundamentals & Reconnaissance vs. Scanning
- Active vs. Passive Reconnaissance
- Port Scanning & Vulnerability Scanning
- TCP Connect, SYN, UDP, ACK, FIN & Window Scans
- OS Fingerprinting & Service Enumeration
- Attack Surface Mapping & Vulnerability Assessment
- Best Practices

### 🔍 Nmap Ultimate Command Cheat Sheet

Master one of the most important tools used in cybersecurity.

Topics include:

- Target Specification & Host Discovery (Ping Sweep)
- Scan Types — TCP SYN, TCP Connect, UDP, ACK, FIN, NULL & Xmas Scans
- Service Version Detection & OS Detection
- NSE Scripts & Timing Templates
- Port Selection & Firewall Evasion
- Output Formats & Performance Optimization

### ⚙️ Nmap NSE Scripts & Netcat

Extend Nmap with automation and learn practical networking using Netcat.

Topics include:

- Nmap Scripting Engine (NSE) — Discovery, Enumeration, Vulnerability & Authentication Scripts
- SMB, HTTP, FTP, DNS & SSL Scripts
- Netcat Basics, Banner Grabbing, Port Listening
- File Transfers, Reverse Shells, Bind Shells
- Network Troubleshooting

### 📦 Tcpdump

Learn command-line packet capture and analysis.

Topics include:

- Packet Capture, Interfaces, Capture Files
- Berkeley Packet Filter (BPF) — Protocol, Host, Port & TCP Flag Filters
- Reading PCAP Files & Packet Analysis
- Troubleshooting, Security Monitoring & Traffic Investigation

### 🦈 Wireshark

Perform deep packet inspection and network traffic analysis.

Topics include:

- Wireshark Interface & Packet Capture
- PCAP Analysis, Capture Filters, Display Filters
- Packet Dissection & Protocol Analysis
- OSI Layer Mapping & Packet Navigation
- Expert Information & Statistics
- Network Troubleshooting & Digital Forensics

### 🖥 Enum4linux

Enumerate Windows and Samba environments.

Topics include:

- Null Sessions, User & Group Enumeration
- Password Policies & Share Enumeration
- RID Cycling & Machine Information
- SMB Enumeration & Enum4linux-ng
- Active Directory Reconnaissance

### 📁 SMBMap

Discover SMB shares and permissions.

Topics include:

- Share Enumeration & Access Levels
- Recursive Listing, File Download, File Upload
- Command Execution & Authentication
- Null Sessions & Writable Share Discovery

### 🌳 SNMP, LDAP & RPC Enumeration

Learn how to enumerate enterprise environments.

**SNMP** — Community Strings, SNMP Versions, OIDs, SNMPWalk, SNMPCheck, OneSixtyOne, Device Enumeration

**LDAP** — Active Directory Enumeration, Domain Objects, Users, Groups, Organizational Units, LDAPSearch

**RPC** — RPC Enumeration, `rpcclient`, `rpcinfo`, Windows Services, Domain Information

### 🔢 Common Ports

Master the most important TCP and UDP ports used during penetration testing.

Topics include:

- File Transfer, Web & Email Ports
- Active Directory & Database Ports
- Network Infrastructure & Remote Administration Ports
- Linux Services, Windows Services & UDP Services
- Port-to-Service Mapping & Enumeration Workflow

### 🌐 DNS Penetration Testing

Topics include:

- DNS Reconnaissance & Banner Grabbing
- DNS Enumeration & Zone Transfers (AXFR)
- Subdomain Enumeration & DNSSEC
- DNS Tunneling & Cache Snooping
- Version Detection, DNS Spoofing, Detection & Remediation

### 📂 FTP Penetration Testing

Topics include:

- Banner Grabbing & Anonymous Login
- Directory Enumeration & FTP Features
- Credential Attacks & Brute Force
- FTP Bounce & File Upload
- Post Exploitation, Evidence Collection & Hardening

### 📁 NFS Penetration Testing

Topics include:

- RPC Discovery & Showmount
- Export Enumeration & Mounting Shares
- `no_root_squash` & Writable Exports
- UID/GID Manipulation
- Privilege Escalation, Persistence & Hardening

### 🖥 SMB Penetration Testing

Topics include:

- SMB Enumeration & Null Sessions
- SMB Signing & Share Discovery
- User Enumeration — SMBMap, Enum4linux
- MS17-010 & SMBGhost
- Credential Attacks, Relay Attacks & Hardening

### 📧 SMTP Penetration Testing

Topics include:

- SMTP Enumeration & Banner Grabbing
- EHLO Enumeration, VRFY, EXPN
- User Enumeration & Open Relay Testing
- SPF, DKIM, DMARC
- Mail Spoofing & Hardening

### 📡 SNMP Penetration Testing

Topics include:

- Community String Discovery
- SNMPWalk, SNMPGet, SNMPCheck, OneSixtyOne
- MIB Enumeration & Device Configuration
- Routing Tables & Read/Write Communities
- Hardening

### 📬 IMAP & POP3 Penetration Testing

Topics include:

- IMAP & POP3 Enumeration
- Authentication Methods — CAPABILITY, NTLM
- Mailbox Enumeration & Credential Attacks
- Password Spraying & Email Harvesting
- TLS Security & Hardening

---

## 🛠 Skills You'll Learn

- Network Reconnaissance & Active Enumeration
- Port Scanning & Service Enumeration
- Vulnerability Identification
- Nmap Mastery & NSE Automation
- Netcat Usage
- Packet Analysis — Tcpdump & Wireshark
- SMB, Active Directory, SNMP, LDAP & RPC Enumeration
- Common Network Services
- Protocol-Specific Security Testing — DNS, FTP, SMB, NFS, SMTP, IMAP/POP3
- Network Attack Surface Assessment

---

## 📁 Folder Structure

```text
04_Network_Scanning_&_Enumeration
│
├── 01. Network Scanning for Ethical Hacking Cybersecurity.pdf
├── 02. Nmap Ultimate Command Cheatsheet.pdf
├── 03. Nmap · NSE Scripts · Netcat — Active Network Recon & Exploitation Toolkit.pdf
├── 04. Tcpdump — Study Guide.pdf
├── 05. Wireshark Capture & Display Filters.pdf
├── 06. Enum4linux — Study Guide.pdf
├── 07. SMBMap — Study Guide.pdf
├── 08. SNMP · LDAP · RPC Enumeration — Study Guide.pdf
├── 09. Common Ports — Study Guide.pdf
├── 10. DNS Penetration Test.pdf
├── 11. FTP Penetration Test.pdf
├── 12. NFS Penetration Test.pdf
├── 13. SMB Penetration Test.pdf
├── 14. SMTP Penetration Test.pdf
├── 15. SNMP Penetration Test.pdf
├── 16. IMAP-POP3 Penetration Test.pdf
└── README.md
```

---

## 🚀 Why Learn These Topics?

Network scanning and enumeration form the foundation of every penetration test and security assessment. Before identifying vulnerabilities or exploiting weaknesses, security professionals must first discover systems, services, protocols, and configurations exposed within the target environment.

By mastering these topics, you'll be able to:

- Perform effective host discovery and port scanning
- Enumerate services and operating systems
- Master Nmap and its scripting engine (NSE)
- Analyze network traffic using Tcpdump and Wireshark
- Enumerate SMB, LDAP, RPC, and SNMP services
- Identify exposed network services and common misconfigurations
- Conduct protocol-specific penetration testing for DNS, FTP, NFS, SMB, SMTP, SNMP, IMAP, and POP3
- Understand common attack paths and service-specific security risks
- Strengthen reconnaissance and attack surface assessment skills for real-world engagements

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

⭐ **If you find these notes helpful, consider giving this repository a Star to support the project and help others learn cybersecurity!**
