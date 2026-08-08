# 🔐 Password Attacks

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Level](https://img.shields.io/badge/level-intermediate-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

> A comprehensive collection of notes covering **online and offline password attacks**, password cracking techniques, hash types, wordlists, rainbow tables, password spraying, and credential stuffing. This section focuses on how password-based authentication can be assessed during authorized security testing and how different attack techniques exploit different weaknesses.

---

## 📖 Table of Contents

- [Recommended Learning Path](#-recommended-learning-path)
- [Topics Covered](#-topics-covered)
  - [Hydra — Network Login Brute-Forcing](#-hydra--network-login-brute-forcing)
  - [John the Ripper](#-john-the-ripper)
  - [Hashcat](#-hashcat)
  - [Wordlists](#-wordlists)
  - [Hash Types](#-hash-types)
  - [Rainbow Tables](#-rainbow-tables)
  - [Password Spraying](#-password-spraying)
  - [Credential Stuffing](#-credential-stuffing)
- [Password Attack Comparison](#️-password-attack-comparison)
- [Online vs. Offline Password Attacks](#-online-vs-offline-password-attacks)
- [How These Topics Connect](#-how-these-topics-connect)
- [Defensive Measures](#️-defensive-measures)
- [Suggested Lab Practice](#-suggested-lab-practice)
- [Authorization & Ethical Use](#️-authorization--ethical-use)
- [Skills You'll Learn](#-skills-youll-learn)
- [MITRE ATT&CK Techniques Covered](#-mitre-attck-techniques-covered)
- [Folder Structure](#-folder-structure)
- [Author](#-author)
- [License](#-license)

---

## 🎯 Recommended Learning Path

Follow the PDFs in this order (matches the numbering used in the folder):

1. **Hydra** — understand how online authentication attacks work against network services.
2. **John the Ripper** & 3. **Hashcat** — understand offline candidate generation, wordlists, rules, masks, and GPU acceleration.
4. **Wordlists** — learn how the quality and relevance of candidate passwords affects cracking efficiency.
5. **Hash Types** — learn why identifying the correct hash format is critical before attempting to crack it.
6. **Rainbow Tables** — understand the storage/computation trade-off and why salting makes rainbow tables ineffective against modern password storage.
7. **Password Spraying** — understand the difference between traditional brute force and one-password-many-user attacks.
8. **Credential Stuffing** — understand how previously compromised credentials can become valid against unrelated services because of password reuse.

---

## 📂 Topics Covered

### 🚨 Hydra — Network Login Brute-Forcing

**Hydra (THC-Hydra)** is a parallelized network login cracker designed to test authentication mechanisms across numerous network protocols. It sits between reconnaissance and initial access by taking discovered services, usernames, and password candidates and testing them against a login mechanism.

**Topics include:** Hydra architecture and core mechanics, network authentication attacks, username/password lists, protocol-specific modules (SSH, FTP, HTTP, SMB, RDP, VNC, MySQL, MSSQL, PostgreSQL, POP3/IMAP/SMTP), single credential testing, username + password list combinations, thread tuning, session resuming, HTTP form-based authentication, success/failure conditions, authentication rate limiting, account lockout, credential stuffing vs. brute force, and Hydra's ATT&CK/CWE/OWASP mapping.

**Important Options**

```text
-l <username>      Single username
-L <userlist>      Username list
-p <password>      Single password
-P <passlist>      Password list
-C <file>          Combined username:password file
-t <n>             Number of parallel tasks
-s <port>          Non-default port
-v / -V            Verbose output
-f                 Stop after first valid pair
-F                 Stop after first valid pair across targets
-o <file>          Save results
-R                 Resume interrupted session
```

Hydra's `-t` concurrency setting represents a trade-off between speed, service stability, and detection. The guide highlights lower concurrency for services such as SSH, SMB, and RDP, and higher concurrency for many HTTP forms.

**HTTP Form Authentication:** HTTP form attacks are more complicated because a failed authentication request may still return an HTTP `200` response — Hydra needs a reliable failure or success indicator to determine whether credentials worked.

### 🔑 John the Ripper

**John the Ripper (JtR)** is an offline password-cracking tool that takes a password hash and attempts to recover the original plaintext by generating candidates, hashing them using the correct algorithm and parameters, and comparing the result with the target hash.

**Single Crack Mode** — uses information associated with the account (usernames, names) to generate likely candidates.
```bash
john --single --format=raw-md5 hashes.txt
```

**Wordlist Mode** — tests candidates from a supplied wordlist.
```bash
john --wordlist=rockyou.txt --format=raw-md5 hashes.txt
```

**Rule-Based Cracking** — applies transformations to wordlist entries (capitalization, character substitutions, leetspeak, appending numbers/symbols, reversing words).
```bash
john --wordlist=rockyou.txt --rules=best64 --format=raw-md5 hashes.txt
```

**Incremental Mode** — attempts character combinations exhaustively; search space grows exponentially with password length and character-set size.
```bash
john --incremental=Digits --format=raw-md5 hashes.txt
```

**Mask Mode** — uses a known password structure to reduce the search space (e.g. uppercase + 4 lowercase + 4 digits, modeling structures like `Snake2024` or `Tiger1987`).
```bash
john --mask='?u?l?l?l?l?d?d?d?d' --format=raw-md5 hashes.txt
```

**Hybrid Mode** — combines wordlists with masks.
```bash
john --wordlist=rockyou.txt --mask='?w?d?d?d?d' --format=raw-md5 hashes.txt
```

### 🎮 Hashcat

**Hashcat** performs the same fundamental candidate → hash → comparison process as JtR but is designed around a **GPU-first architecture**, making it particularly useful for large-scale cracking of fast, highly parallelizable hashes.

| Mode | Name | Purpose |
|---|---|---|
| `-a 0` | Straight | Dictionary attack |
| `-a 1` | Combination | Combine two wordlists |
| `-a 3` | Mask | Structured brute force |
| `-a 6` | Hybrid | Wordlist + appended mask |
| `-a 7` | Hybrid | Prepended mask + wordlist |
| `-a 9` | Association | Candidate generation using associated metadata |

```bash
# Straight attack, with rules
hashcat -a 0 -m 0 hashes.txt rockyou.txt -r best64.rule

# Combination attack (e.g. dragon+castle, tiger+fortress)
hashcat -a 1 -m 0 hashes.txt wordlist1.txt wordlist2.txt

# Mask attack — ?l lowercase, ?u uppercase, ?d digit, ?s special, ?a all printable ASCII
hashcat -a 3 -m 0 hashes.txt ?u?l?l?l?l?d?d?d?d

# Hybrid: wordlist + suffix, or mask + wordlist
hashcat -a 6 -m 0 hashes.txt rockyou.txt ?d?d?d?d
hashcat -a 7 -m 0 hashes.txt ?d?d?d?d rockyou.txt

# Benchmarking
hashcat -b
hashcat -b -m 1000
```

The guide emphasizes that fast hashes such as MD5 and NTLM can be processed dramatically faster than deliberately slow password-hashing algorithms such as bcrypt and Argon2.

### 📖 Wordlists

A **wordlist is the candidate-generation component** of a password-cracking workflow. Its effectiveness depends primarily on how closely it represents the password-selection behavior of the target population.

**Common wordlists:**
- **rockyou.txt** — a widely used breach-derived baseline containing approximately 14 million passwords from the 2009 RockYou breach; captures real password patterns rather than only dictionary words.
- **SecLists** — a maintained collection of password lists, username lists, web-content lists, fuzzing lists, and discovery lists.
- **Probable Wordlists** — frequency-ranked lists that prioritize candidates by observed real-world password frequency.
- **Language/Region-Specific Lists** — improved by considering language, regional terminology, local slang, cultural references, and regional breach data.

**Custom Wordlist Generation**
```bash
# Merge wordlists
cat rockyou.txt seclists-list2.txt custom-terms.txt | sort -u > combined.txt

# CeWL — extract vocabulary from an authorized test organization's website
cewl -d 2 -m 5 -w custom-wordlist.txt https://target.example

# Evaluate a wordlist
wc -l wordlist.txt
awk '{print length}' wordlist.txt | sort -n | uniq -c
```

CeWL output can include company terminology, product names, department names, executive names, and organization-specific terms. When evaluating a wordlist, consider entry count, password-length distribution, duplicate entries, encoding consistency, target password policy, and relevance to the target population.

### 🧬 Hash Types

A cryptographic hash converts input data into a fixed-length digest. The guide distinguishes between **general-purpose fast hashes** and **password-hardening functions**.

> **Cryptographic strength and password-cracking resistance are different properties.** Fast hashes can be excellent for integrity checking while still being inappropriate for password storage, because attackers can calculate enormous numbers of guesses per second.

**Fast / General-Purpose Hashes**

| Hash | Hashcat Mode | JtR Format |
|---|---:|---|
| MD5 | `-m 0` | `raw-md5` |
| SHA-1 | `-m 100` | `raw-sha1` |
| SHA-256 | `-m 1400` | `raw-sha256` |
| SHA-512 | `-m 1700` | `raw-sha512` |
| NTLM | `-m 1000` | `nt` |
| Kerberos TGS | `-m 13100` | `krb5tgs` |

**Password-Hardening Algorithms**

| Algorithm | Hashcat Mode | JtR Format |
|---|---:|---|
| bcrypt | `-m 3200` | `bcrypt` |
| scrypt | `-m 8900` | `scrypt` |
| Argon2 | `-m 34000` | `argon2` |
| PBKDF2-SHA256 | `-m 10900` | `pbkdf2-hmac-sha256` |
| sha512crypt | `-m 1800` | `sha512crypt` |

> Hashcat mode numbers can shift slightly between versions — worth a quick `hashcat --help` check against your installed version before relying on these in a real engagement.

**Salting:** a unique random salt causes identical passwords to produce different hashes, preventing precomputed rainbow tables from being reused across users.

```text
Without salt:  password → SAME hash
With salts:    password + salt_A → hash_A
               password + salt_B → hash_B
```

### 🌈 Rainbow Tables

Rainbow tables use a **precomputation strategy** rather than generating every password candidate from scratch during each attack. Instead of storing every plaintext/hash pair, they use chains of Plaintext → Hash → Reduction → Plaintext → Hash → ... Only the starting and ending values of each chain are stored; intermediate values are reconstructed on demand.

**Core concepts:** precomputation, hash chains, reduction functions, chain endpoints, lookup, chain reconstruction, storage-vs-computation trade-off.

**Tooling:** RainbowCrack (`rtgen`, `rtsort`, `rcrack`), Ophcrack

```bash
rtgen md5 loweralpha-numeric 1 7 0 3800 33554432 0
rtsort *.rt
rcrack *.rt -h <hash>
```

**Why salting defeats rainbow tables:** for an unsalted hash, `hash("password123")` always produces the same result, so a precomputed table can be reused. With unique salts, a table would need to account for every possible salt, making precomputation impractical. As a result, rainbow tables are largely obsolete against properly salted modern password-storage schemes — GPU-based cracking has shifted the trade-off toward on-demand computation for many legacy unsalted hashes.

### 🎯 Password Spraying

**Password spraying** is an online authentication attack that attempts a small number of likely passwords against many accounts, rather than many passwords against one account — trading **depth for breadth**.

```text
Offline cracking:    ONE account  → MANY passwords
Password spraying:   MANY accounts → FEW passwords
```

**Workflow:** enumerate usernames → determine the authentication/lockout policy → select a small set of likely passwords → attempt one password across the account population → respect the lockout observation window → repeat with the next candidate. The guide emphasizes a **small, high-probability password set**, rather than a massive wordlist.

**Tools discussed:** Kerbrute, NetExec, MSOLSpray, O365Spray, and provider-specific equivalents.

**MITRE ATT&CK:** `T1110.003 — Password Spraying`

### 🔄 Credential Stuffing

**Credential stuffing** differs fundamentally from password spraying: instead of guessing passwords, it uses **known username:password pairs** obtained from previous compromises and tests whether users have reused those credentials on another service.

```text
Password Spraying:    Many usernames + few guessed passwords → exploits password weakness
Credential Stuffing:  Known username:password pairs, different target service → exploits password reuse
```

**Credential sources discussed:** previous breach compilations, credentials recovered through authorized offline cracking, combolists, infostealer-derived credential data. The guide notes that offline cracking output can itself become an input to credential-stuffing datasets.

**Why it works:** the target's password does not need to be weak — a strong password compromised at Service A becomes a valid credential at Service B if reused. The exploited weakness is **credential reuse**, not password weakness.

**Automation concepts discussed:** combolist processing, target-specific request configuration, cookie/token handling, success/failure classification, multi-threaded processing, proxy infrastructure, detection considerations.

**MITRE ATT&CK:** `T1110.004 — Credential Stuffing`

---

## ⚔️ Password Attack Comparison

| Attack | Input | Attack Type | Main Weakness |
|---|---|---|---|
| **Hydra Brute Force** | Username/password candidates | Online | Weak authentication controls |
| **JtR** | Password hashes | Offline | Weak/recoverable passwords |
| **Hashcat** | Password hashes | Offline | Weak/recoverable passwords |
| **Wordlist Attack** | Candidate passwords | Offline | Predictable password choices |
| **Rainbow Tables** | Precomputed chains | Offline | Unsalted hashes |
| **Password Spraying** | Many usernames + few passwords | Online | Weak/common passwords |
| **Credential Stuffing** | Known username:password pairs | Online | Password reuse |

---

## 🧠 Online vs. Offline Password Attacks

**🌐 Online attacks** (Hydra, Password Spraying, Credential Stuffing) interact directly with a live authentication service and are affected by rate limiting, account lockout, MFA, CAPTCHA, IP reputation, detection systems, and authentication monitoring.

**💾 Offline attacks** (John the Ripper, Hashcat, Rainbow Tables) assume the attacker already possesses password hashes or other credential material, so they aren't directly constrained by the target's login rate limits — the major variables instead become Hash Cost × Candidate Quality × Available Compute. Fast hashes (MD5, SHA-family, NTLM) can be attacked at very high rates, while bcrypt, scrypt, Argon2, and highly iterated PBKDF2 are intentionally much slower.

---

## 🔗 How These Topics Connect

```text
                 PASSWORD ATTACKS
                        │
          ┌─────────────┴─────────────┐
          │                           │
       ONLINE                      OFFLINE
          │                           │
    ┌─────┼─────┐               ┌─────┼─────┐
    │     │     │               │     │     │
  Hydra Spray Stuffing          JtR Hashcat Rainbow
    │     │     │               │     │     │
    └─────┴─────┘               └─────┴─────┘
          │                           │
   Authentication                Hash Recovery
      Attacks                         │
                              ┌───────┴───────┐
                              │               │
                         Wordlists        Hash Types
                              │               │
                              └───────┬───────┘
                                      │
                                  Candidates
                                      │
                                      ▼
                               Password Recovery
```

---

## 🛡️ Defensive Measures

**Strong password storage** — use deliberately slow and/or memory-hard hashing functions (Argon2id, bcrypt, scrypt, PBKDF2) with unique salts so identical passwords don't produce identical hashes.

**Avoid fast hashes for password storage** — general-purpose fast hashes (MD5, SHA-1, SHA-256, SHA-512) are useful for integrity checking but make large-scale password guessing much easier when used for password storage.

**Multi-Factor Authentication (MFA)** — an additional authentication factor beyond the password; particularly important against spraying, stuffing, guessing, and reuse. The credential-stuffing guide specifically identifies MFA as a control that remains effective even when credentials have been compromised elsewhere.

**Rate limiting** — per-account and per-IP rate limiting, adaptive throttling, login anomaly detection, progressive delays.

**Account lockout / protection** — detect excessive failed attempts while balancing security against denial-of-service risk.

**Breached password screening** — check new passwords against known-compromised password corpora rather than relying solely on complexity rules.

**Prevent password reuse** — unique passwords per service; credential stuffing demonstrates why a password compromised at one service can become valid elsewhere.

---

## 🧪 Suggested Lab Practice

Practice these techniques only against systems you own or environments where you have explicit authorization.

```text
1. Create synthetic password hashes
2. Identify the hash format
3. Test JtR --single
4. Test a small wordlist
5. Add mangling rules
6. Compare JtR and Hashcat
7. Experiment with masks
8. Compare fast vs. slow hash performance
9. Study rainbow-table behavior using unsalted test hashes
10. Build a controlled login lab
11. Study Hydra authentication behavior
12. Study password spraying using test accounts
13. Build a credential-reuse lab
```

---

## 🛡️ Authorization & Ethical Use

All password-attack techniques in this section should be used only in your own lab, CTF environments, vulnerable-by-design machines, authorized penetration tests, or security assessments with explicit written permission. **Never test credentials against systems or accounts without authorization.**

---

## 🏆 Skills You'll Learn

| Topic | Key Idea |
|---|---|
| Hydra | Online network login attacks |
| John the Ripper | Offline CPU-oriented cracking with broad format support |
| Hashcat | GPU-first cracking |
| Wordlists | Candidate password source |
| Rules & Masks | Transform candidates / target a known password structure |
| Hash Types | Determine cracking method and cost |
| Salting | Makes identical passwords produce different hashes |
| Rainbow Tables | Precomputed hash/reduction chains |
| Password Spraying | Few passwords against many accounts |
| Credential Stuffing | Known credentials against another service |
| MFA | Strong defense against credential-based attacks |
| Argon2id | Modern password-hardening choice |

You'll also come away understanding network authentication attacks, hash identification, wordlist selection & custom generation, password mangling, mask/hybrid attacks, hash salting, and defensive controls including rate limiting, account lockout, MFA, and breached-password screening.

---

## 📌 MITRE ATT&CK Techniques Covered

```text
T1110 — Brute Force
├── T1110.001 — Password Guessing
├── T1110.002 — Password Cracking
├── T1110.003 — Password Spraying
└── T1110.004 — Credential Stuffing
```

The PDFs map password-cracking material to **T1110.002**, while Hydra, password spraying, and credential stuffing cover the corresponding online authentication sub-techniques.

---

## 📁 Folder Structure

```text
08_Password_Attacks
│
├── 01. Hydra — Network Login Brute-Forcing.pdf
├── 02. John the Ripper.pdf
├── 03. Hashcat.pdf
├── 04. Wordlists.pdf
├── 05. Hash Types.pdf
├── 06. Rainbow Tables.pdf
├── 07. Password Spraying.pdf
├── 08. Credential Stuffing.pdf
└── README.md
```

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

⭐ **If this Cybersecurity Knowledge Base helps you learn cybersecurity, consider giving the repository a Star and sharing it with others learning penetration testing, VAPT, ethical hacking, red teaming, SOC & blue team work, network security, or web application security.**

**Learn → Practice → Document → Improve → Share 🚀**
