# ⚔️ Metasploit & Exploitation

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Level](https://img.shields.io/badge/level-intermediate-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

> Learn how to use the **Metasploit Framework** for vulnerability exploitation, payload generation, post-exploitation, privilege escalation, and network pivoting. This section covers the complete exploitation workflow, Meterpreter operations, payload creation with **msfvenom**, Metasploit module types, and pivoting techniques used during professional penetration tests. The guides explain how Metasploit integrates reconnaissance, exploitation, post-exploitation, and lateral movement into a single modular framework.

---

## 📖 Table of Contents

- [Recommended Learning Path](#-recommended-learning-path)
- [Topics Covered](#-topics-covered)
  - [Metasploit Framework](#-metasploit-framework)
  - [Meterpreter](#-meterpreter)
  - [msfvenom](#-msfvenom)
  - [Exploit, Auxiliary & Post Modules](#️-exploit-auxiliary--post-modules)
  - [Pivoting](#-pivoting)
- [Skills You'll Learn](#-skills-youll-learn)
- [Folder Structure](#-folder-structure)
- [Why Learn Metasploit?](#-why-learn-metasploit)
- [Author](#-author)
- [License](#-license)

---

## 🎯 Recommended Learning Path

Study the topics in the following order (matches the numbering used in the folder):

1. Metasploit Framework
2. Meterpreter
3. msfvenom
4. Exploit Modules — from `04. Exploit - Auxiliary - Post Modules.pdf`
5. Auxiliary Modules — same document as above
6. Post Modules — same document as above
7. Pivoting

---

## 📂 Topics Covered

### 🚀 Metasploit Framework

Learn the architecture and workflow of the Metasploit Framework, including how exploits, payloads, auxiliary modules, post modules, encoders, and databases work together during a penetration test.

Topics include:

- Framework Architecture & PostgreSQL Database
- msfconsole & Module Types
- Exploit Workflow & Payload Selection
- Reverse vs. Bind Payloads
- Staged vs. Single Payloads
- Session Management
- Local Exploit Suggester
- Best Practices

### 💻 Meterpreter

Understand Meterpreter's in-memory architecture and learn how to perform professional post-exploitation tasks after gaining a session. Covers why Meterpreter differs from a traditional reverse shell, along with file operations, process migration, credential access, privilege escalation, and persistence.

Topics include:

- Meterpreter Architecture & In-Memory Execution
- Stager vs. Stage & Session Creation
- System & Network Enumeration
- Process Migration & File Upload/Download
- Native Shell Access
- Credential Dumping (Kiwi)
- Privilege Escalation & Persistence
- Screen Capture, Keylogging, Webcam Access
- Session Management

### 📦 msfvenom

Learn how to generate standalone payloads for multiple platforms and delivery methods, including payload generation, encoding, templates, output formats, and integration with Metasploit handlers.

Topics include:

- msfvenom Basics & Payload Syntax/Naming Convention
- Reverse TCP & Reverse HTTPS Payloads
- Linux, Windows, Android, PHP, PowerShell Payloads
- Raw Shellcode & Payload Encoders
- Bad Characters & Template Injection
- Multi Handler
- AV Detection Limitations & Payload Delivery Techniques

### ⚙️ Exploit, Auxiliary & Post Modules

Understand how Metasploit modules are structured and when to use Exploit, Auxiliary, and Post modules during an engagement — module rankings, target selection, vulnerability verification, scanners, brute-force modules, and post-exploitation automation.

**Exploit Modules**
Module Structure, Module Ranking, Target Selection, Check Method, Payload Delivery, Remote/Local/Client-Side/Web Exploits

**Auxiliary Modules**
Port Scanning, SMB/FTP Enumeration, SSH/HTTP/MySQL Login, Vulnerability Scanners, Password Spraying, Brute Force Modules, DoS Modules, Fuzzing Modules

**Post Modules**
Privilege Escalation, Credential Collection, Enumeration, Persistence, Loot Collection, Local Exploit Suggester, Cleanup Operations

### 🌐 Pivoting

Learn how to move laterally through internal networks after compromising an initial host — Metasploit routing, SOCKS proxies, SSH tunneling, Chisel, and multi-hop pivoting techniques used during internal penetration tests.

Topics include:

- Internal Network Discovery & Pivoting Concepts
- Autoroute & Port Forwarding
- SOCKS Proxy & ProxyChains
- SSH Local, Dynamic & Remote Port Forwarding
- Chisel & Double Pivoting
- Route Management & Internal Enumeration
- Lateral Movement & Pivot Detection Checklist

---

## 🛠 Skills You'll Learn

- Metasploit Framework & msfconsole
- Exploit Development Workflow
- Meterpreter Operations & Session Management
- Payload Selection — Reverse & Bind Shells
- Payload Generation with msfvenom
- Exploit, Auxiliary & Post Modules
- Vulnerability Exploitation & Privilege Escalation
- Credential Harvesting & Persistence
- Process Migration
- Port Forwarding, SOCKS Proxy, SSH Tunneling, ProxyChains
- Pivoting & Lateral Movement
- Internal Network Enumeration
- Professional Penetration Testing Workflow

---

## 📁 Folder Structure

```text
07_Metasploit_&_Exploitation
│
├── 01. Metasploit Framework — Architecture, Workflow & Post-Exploitation.pdf
├── 02. Meterpreter.pdf
├── 03. msfvenom.pdf
├── 04. Exploit - Auxiliary - Post Modules.pdf
├── 05. Pivoting.pdf
└── README.md
```

---

## 🚀 Why Learn Metasploit?

The Metasploit Framework is one of the most widely used exploitation frameworks in cybersecurity. It enables security professionals to validate vulnerabilities, automate exploitation workflows, perform post-exploitation tasks, and simulate real-world attack scenarios in authorized environments.

Mastering Metasploit helps you:

- Validate discovered vulnerabilities
- Perform efficient exploitation
- Generate custom payloads
- Manage multiple sessions
- Escalate privileges
- Harvest credentials
- Conduct internal penetration tests
- Pivot through segmented networks
- Automate post-exploitation tasks
- Prepare for certifications such as eJPT, PNPT, CEH Practical, OSCP, CRTP, CRTO, and real-world penetration testing engagements

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
