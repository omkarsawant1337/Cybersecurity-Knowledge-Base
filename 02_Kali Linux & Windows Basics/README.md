# 🖥️ Kali Linux & Windows Basics

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Level](https://img.shields.io/badge/level-beginner--to--intermediate-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

> Master the operating systems used daily by cybersecurity professionals. Learn Linux and Windows fundamentals, command-line navigation, scripting, permissions, system administration, and security concepts required for ethical hacking, penetration testing, SOC analysis, and Active Directory.

---

## 📖 Table of Contents

- [Recommended Learning Path](#-recommended-learning-path)
- [Linux Fundamentals](#-linux-fundamentals)
  - [Essential Linux Commands](#-essential-linux-commands)
  - [Bash Scripting](#-bash-scripting)
  - [Cron Jobs](#-cron-jobs)
  - [Linux File Permissions](#-linux-file-permissions)
  - [Kali Linux Tools](#-kali-linux-tools)
- [Windows Fundamentals](#-windows-fundamentals)
  - [Windows Commands](#-windows-commands)
  - [PowerShell Basics](#-powershell-basics)
  - [Windows File Permissions](#-windows-file-permissions)
  - [Windows Registry](#-windows-registry)
  - [Windows Services](#-windows-services)
- [Skills You'll Learn](#-skills-youll-learn)
- [Folder Structure](#-folder-structure)
- [Why Learn These Topics?](#-why-learn-these-topics)
- [Author](#-author)
- [License](#-license)

---

## 🎯 Recommended Learning Path

Follow this order to build a strong foundation:

1. 50 Essential Linux Commands
2. Linux File Permissions
3. Bash Scripting
4. Cron Jobs
5. Top 50 Kali Linux Tools
6. Top 200 Windows Commands
7. PowerShell Basics
8. Windows File Permissions
9. Windows Registry
10. Windows Services

---

## 🐧 Linux Fundamentals

Covers the Linux file system, directory navigation, file management, process management, package management, networking commands, file searching, compression & archiving, SSH & remote access, text processing, system monitoring, and environment variables.

### 💻 Essential Linux Commands

The companion PDF (`50 Essential Linux Commands...pdf`) walks through 50 commonly used Linux commands. Highlights covered here include:

`ls` · `cd` · `pwd` · `mkdir` · `rm` · `cp` · `mv` · `cat` · `grep` · `chmod` · `chown` · `sudo` · `apt` · `ps` · `top` · `kill` · `df` · `du` · `ssh` · `scp` · `tar` · `gzip` · `curl` · `wget` · `ping` · `netstat` · `history` · `find` · `locate` · `sed` · `awk` · `cut` · `diff` · `sort` · `tail` · `head` · `ssh-keygen` · `wc` · `ln` · `alias`

### 📜 Bash Scripting

Learn Linux automation using Bash. Topics include:

- Shell Basics, Variables, Command Substitution, Arguments
- Conditional Statements, Loops, Functions, User Input
- Exit Codes, Error Handling, File Operations
- Automation Scripts — Recon Automation, Port Scanning Scripts
- Bash Best Practices

### ⏰ Cron Jobs

Understand Linux task scheduling. Topics include:

- Cron Daemon, Crontab Syntax, Schedule Expressions, Special Characters
- Editing Crontab — User Cron Jobs, System Cron Jobs
- Scheduling Scripts, Automated Backups, Scheduled Recon, Report Generation
- Security Considerations & Cron-based Privilege Escalation

### 🔒 Linux File Permissions

Understand Linux permission management. Topics include:

- File Types, Read/Write/Execute, Owner, Group, Others
- Numeric & Symbolic Permissions
- `chmod`, `chown`, `chgrp`, Recursive Permissions
- SUID, SGID, Sticky Bit
- Permission Auditing & Privilege Escalation Concepts

### 🛠 Kali Linux Tools

Explore 50 essential Kali Linux tools, categorized by phase:

| Category | Tools |
|---|---|
| Information Gathering | Nmap, Recon-ng, Maltego, theHarvester, Amass, Dnsenum, Whois, Netdiscover, Shodan CLI |
| Vulnerability Assessment | Nessus, OpenVAS, Nikto, Lynis, Wapiti, Skipfish |
| Exploitation | Metasploit, SQLmap, BeEF, RouterSploit, SET |
| Web Application Testing | Burp Suite, OWASP ZAP, Gobuster, Dirb, Dirbuster, WhatWeb, Wfuzz |
| Password Attacks | Hydra, John the Ripper, Hashcat, Medusa, CeWL, Crunch |
| Wireless Security | Aircrack-ng, Reaver, Wifite, Kismet, Wifiphisher |
| Network Analysis | Wireshark, Bettercap, Ettercap, dsniff |
| Post Exploitation & Forensics | Netcat, BloodHound, CrackMapExec, Evil-WinRM, PowerSploit, Volatility, Autopsy, Empire, Chisel, Socat |

---

## 🪟 Windows Fundamentals

Covers Command Prompt (CMD), PowerShell, the Windows file system, NTFS permissions, the registry, Windows services and service accounts, system information, networking commands, user management, and process management.

### ⌨ Windows Commands

The companion PDF (`Top 200 Most Useful Windows Commands.pdf`) covers 200 commands across categories. Highlights covered here include:

**File Management:** `dir` · `cd` · `copy` · `move` · `del` · `ren` · `tree` · `attrib` · `find` · `findstr`

**Networking:** `ipconfig` · `ping` · `tracert` · `nslookup` · `netstat` · `arp` · `route` · `hostname` · `getmac` · `net use`

**System Information:** `systeminfo` · `tasklist` · `taskkill` · `whoami` · `driverquery` · `wmic` · `sfc` · `chkdsk` · `diskpart`

### ⚡ PowerShell Basics

Learn Windows automation using PowerShell. Topics include:

- Cmdlets & Verb-Noun Syntax
- Variables, Arrays, Hashtables, Objects, Pipelines
- `Where-Object`, `Select-Object`, `Get-Help`, `Get-Command`
- Loops, Functions, PowerShell Scripting
- Object-Based Administration

### 🔐 Windows File Permissions

Understand NTFS security. Topics include:

- NTFS Permissions, ACL, DACL, SACL, SID
- Permission Inheritance — Basic & Advanced Permissions
- `icacls`, `Get-Acl`, Ownership
- Permission Auditing & NTFS Privilege Escalation

### 🗂 Windows Registry

Learn the Windows configuration database. Topics include:

- Registry Structure, Hives, Keys, Values, Value Types
- `HKLM`, `HKCU`, `HKCR`, `HKU`, `HKCC`
- Registry Editor & the `reg` command
- Persistence Mechanisms & Registry Forensics

### ⚙ Windows Services

Understand Windows background services. Topics include:

- Service Control Manager (SCM), Service Accounts, Startup Types
- Service Configuration & Permissions, Service Enumeration
- `sc.exe`, `Get-Service`, `New-Service`, `Start-Service`, `Stop-Service`
- Service Security & Windows Service Privilege Escalation

---

## 🛠 Skills You'll Learn

- Linux Command Line
- Windows Command Line
- PowerShell
- Bash Scripting
- Linux Administration
- Windows Administration
- Task Automation
- System Monitoring
- File System Management
- Permission Management
- Service Management
- Registry Analysis
- Security Hardening
- Privilege Escalation Fundamentals
- Kali Linux Tool Usage
- Reconnaissance Workflow
- Basic System Administration

---

## 📁 Folder Structure

```text
02_Kali Linux & Windows Basics
│
├── 50 Essential Linux Commands for Efficient Command-Line Usage.pdf
├── Bash Scripting.pdf
├── Cron Jobs.pdf
├── Linux File Permissions.pdf
├── PowerShell Basics.pdf
├── The Ultimate Kali Linux Tool Cheat Sheet 50 Tools to Master.pdf
├── Top 200 Most Useful Windows Commands.pdf
├── Windows File Permissions.pdf
├── Windows Registry.pdf
├── Windows Services.pdf
└── README.md
```

---

## 🚀 Why Learn These Topics?

A strong understanding of Linux and Windows is essential for every cybersecurity professional. These operating systems form the backbone of penetration testing, SOC operations, incident response, malware analysis, Active Directory security, and system administration.

Mastering these fundamentals prepares you for advanced topics such as:

- Enumeration & Privilege Escalation
- Active Directory
- Network Security
- Web Application Security
- Vulnerability Assessment
- Digital Forensics & Malware Analysis
- Red Team / Blue Team Operations
- Capture The Flag (CTF)
- OSCP & PNPT Preparation

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
