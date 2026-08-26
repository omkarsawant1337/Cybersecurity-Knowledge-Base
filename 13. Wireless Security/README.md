# 📡 Wireless Security

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Level](https://img.shields.io/badge/level-intermediate-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

A structured collection of wireless security protocols, assessment tools, attack techniques, detection methods, and defensive controls.

> ⚠️ **Authorized-Lab Use Only** — use the techniques in this repository only on systems you own or have explicit, documented permission to assess.

---

## 📖 Table of Contents

- [Topic Overview](#-topic-overview)
- [Learning Objectives](#-learning-objectives)
- [01. Wi-Fi Security Evolution (WEP → WPA → WPA2 → WPA3)](#-01-wi-fi-security-evolution)
- [02. Aircrack-ng](#-02-aircrack-ng)
- [03. Reaver](#-03-reaver)
- [04. Wifite](#-04-wifite)
- [05. Evil Twin Attacks](#️-05-evil-twin-attacks)
- [06. Rogue Access Points](#-06-rogue-access-points)
- [Detection & Response](#-detection--response)
- [Evil Twin vs. Rogue AP](#-evil-twin-vs-rogue-ap)
- [Wireless Attack Landscape](#-wireless-attack-landscape)
- [Defensive Security Recommendations](#-defensive-security-recommendations)
- [Recommended Study Order](#-recommended-study-order)
- [Quick Revision Cheat Sheet](#-quick-revision-cheat-sheet)
- [Key Takeaways](#-key-takeaways)
- [Folder Structure](#-folder-structure)
- [Related Cybersecurity Topics](#-related-cybersecurity-topics)
- [Author](#-author)
- [License](#-license)
- [Legal & Ethical Disclaimer](#️-legal--ethical-disclaimer)

---

## 📚 Topic Overview

| # | Topic | Focus |
|---|---|---|
| 01 | WEP · WPA · WPA2 · WPA3 | Wi-Fi security generations, encryption, authentication, weaknesses |
| 02 | Aircrack-ng | Wireless assessment suite, monitor mode, capture and WPA/WEP assessment |
| 03 | Reaver | WPS security, PIN weakness, lockout behavior and Pixie Dust |
| 04 | Wifite | Automated orchestration of wireless assessment workflows |
| 05 | Evil Twin Attacks | Rogue AP impersonation, client trust and captive portals |
| 06 | Rogue Access Points | Unauthorized APs, wired bridging, detection and prevention |

---

## 🎯 Learning Objectives

After studying this section, you should be able to:

- Explain the evolution from WEP → WPA → WPA2 → WPA3.
- Explain why WEP is completely broken.
- Understand TKIP, CCMP, GCMP, SAE, and OWE.
- Distinguish WPA2-Personal from WPA2-Enterprise.
- Explain WPA2 handshake/password-testing weaknesses.
- Understand how WPA3/SAE changes the offline-guessing model.
- Explain WPS and its PIN-validation weakness.
- Understand the roles of Aircrack-ng's major tools.
- Understand monitor mode, wireless discovery, and targeted capture.
- Explain how Wifite automates existing assessment workflows.
- Understand why an Evil Twin attacks client trust rather than Wi-Fi encryption.
- Distinguish an Evil Twin from the broader rogue-AP category.
- Identify why a rogue AP connected to an internal switch is especially dangerous.
- Understand WIDS/WIPS, 802.1X, port security, RF surveys, and segmentation as defenses.

---

## 📶 01. Wi-Fi Security Evolution

Each Wi-Fi security generation was introduced to address weaknesses in its predecessor.

| Protocol | Introduced | Encryption / Mechanism | Status |
|---|---|---|---|
| WEP | 1997 | RC4 | ❌ Broken |
| WPA | 2003 | RC4 + TKIP | ⚠️ Deprecated |
| WPA2 | 2004 | AES-CCMP | ✅ Widely deployed |
| WPA3 | 2018 | AES-GCMP + SAE | ✅ Current standard |

Legacy protocol support and WPA2 fallback remain important real-world wireless security gaps.

### WEP

Uses RC4, a static shared key, and a 24-bit IV. IV reuse, RC4 key-scheduling weaknesses, and a non-cryptographic CRC-32 integrity mechanism make it structurally insecure. **Status: completely broken.** Key lesson: WEP's weakness is structural, not simply a matter of password strength.

### WPA

An interim replacement for WEP. Introduced TKIP, including per-packet key mixing, sequence protection, and stronger integrity checks, while retaining RC4 for compatibility. Never intended as a long-term solution — now deprecated.

### WPA2

Replaced RC4/TKIP with AES-CCMP.

- **WPA2-Personal** — uses a shared Pre-Shared Key (PSK)/passphrase.
- **WPA2-Enterprise** — uses 802.1X + RADIUS and individual credentials/certificates, providing better accountability and easier credential revocation.

Offline password attacks against captured WPA2-Personal handshakes and the **2017 KRACK** implementation flaw are highlighted. WPA2-Enterprise also depends on correct RADIUS certificate validation.

### WPA3

Introduces **SAE (Simultaneous Authentication of Equals)** for WPA3-Personal, replacing the WPA2-Personal authentication model and resisting the same straightforward offline dictionary attack against a captured exchange.

**Other improvements:** forward secrecy, AES-GCMP, an optional 192-bit WPA3-Enterprise security mode, and **OWE (Opportunistic Wireless Encryption)** for individualized protection on open networks.

Mixed WPA2/WPA3 transitional deployments can retain some WPA2-level weaknesses, since clients may fall back to WPA2.

---

## 🛠️ 02. Aircrack-ng

Aircrack-ng is a suite rather than one program, dividing wireless assessment into distinct stages.

| Tool | Role |
|---|---|
| `airmon-ng` | Enable monitor mode |
| `airodump-ng` | Capture and inspect wireless traffic |
| `aireplay-ng` | Packet injection / traffic-generation techniques |
| `aircrack-ng` | Key-recovery engine |

**Monitor mode** is the foundation of the capture workflow — it allows a wireless adapter to observe frames on the relevant channel regardless of destination.
```text
Wireless Adapter → Monitor Mode → Discovery → Capture → Authorized Assessment
```

**Discovery** — `airodump-ng` can identify BSSID, signal strength, channel, encryption type, ESSID, and associated clients, supporting targeted assessment.

**Targeted capture** — narrow capture to the intended channel and BSSID for a cleaner assessment capture.

**WEP assessment** is a statistical key-recovery problem caused by WEP's weak IV/RC4 design, rather than a normal password-wordlist problem.

**WPA/WPA2 assessment** — for WPA/WPA2-Personal, the workflow depends on capturing a complete 4-way handshake and then testing candidate passphrases offline. Success depends heavily on password quality and candidate coverage. This process does **not** break AES/CCMP — it attempts to recover the passphrase that derives the relevant session key.

**WPA3** — the same captured-handshake offline workflow does not apply to WPA3's SAE exchange, since SAE is designed to resist that attack class.

---

## 📡 03. Reaver

Reaver targets a separate authentication feature — **WPS (Wi-Fi Protected Setup)** — rather than directly attacking the WPA/WPA2 passphrase.

### WPS PIN Weakness

The 8-digit WPS PIN is not validated as one complete value:

| Component | Validation | Search Space |
|---|---|---|
| Digits 1–4 | Separately validated | 10,000 |
| Digits 5–7 | Separately validated | 1,000 |
| Digit 8 | Checksum | No additional entropy |
| **Effective search (two-stage)** | | **~11,000** |

This is substantially smaller than the theoretical 100-million-combination space of an undivided 8-digit PIN.

> **Important:** a strong Wi-Fi passphrase does not eliminate a vulnerable WPS attack path.

**Lockout** — router vendors introduced WPS lockout and rate-limiting policies; thresholds and behavior vary by hardware and firmware.

**Pixie Dust** — an offline technique applicable to certain vulnerable router chipsets with weak/predictable random-number generation.

**Assessment logic**
```text
WPS enabled? → No → Stop
             → Yes → Check status → Assess vulnerability
                                       ├── Pixie Dust
                                       └── Online PIN
                                             → Document result
```
Methodology emphasizes authorization, monitor mode, WPS discovery, lockout evaluation, and documentation.

---

## ⚙️ 04. Wifite

Wifite introduces no new cryptographic attack — its main value is **orchestration and automation** of workflows already provided by tools such as Aircrack-ng and Reaver.

**Decision model**
```text
Discover Networks → Classify Configuration
  ├── WEP              → Aircrack-ng-style assessment
  ├── WPA/WPA2 + WPS    → Reaver-style assessment
  ├── WPA/WPA2 no WPS   → Handshake + offline assessment
  └── WPA3              → Generally skipped/flagged
```
Wifite classifies encryption, WPS state, signal strength, and client activity before selecting an assessment approach.

**Automation vs. manual control**

| Wifite | Aircrack-ng / Reaver |
|---|---|
| Automated orchestration | Fine-grained control |
| Multi-target workflow | Individual target focus |
| Automatic classification | Explicit operator decisions |
| Convenient for broad authorized assessments | Better for difficult targets |
| Less manual intervention | More timing/retry control |

Its main advantage is consistent throughput across multiple targets, not a new capability.

---

## 🕸️ 05. Evil Twin Attacks

An Evil Twin is fundamentally different from Wi-Fi key cracking — it exploits a **trust-model weakness**: an SSID does not inherently provide a cryptographic binding to one verified physical access point.

```text
Legitimate AP                          Evil Twin AP
SSID: "CoffeeShop-WiFi"      vs.       SSID: "CoffeeShop-WiFi"
BSSID: AA:BB:CC:11:22:33               BSSID: DD:EE:FF:44:55:66
```

Many clients can automatically reconnect to a previously known SSID without inherently proving that the broadcasting AP is legitimate.

> **Key lesson:** an Evil Twin wins through trust and impersonation, not cryptographic cracking.

**Typical components:** rogue AP, matching SSID, optional BSSID cloning, DNS/DHCP services, captive portal, potential client-disconnection techniques.

**Captive portal attack chain**
```text
Victim connects → Rogue AP provides network services →
Requests redirected to fake portal → Victim enters information →
Credentials / data captured
```
Captured information may include network passwords, personal information, or payment details depending on the portal.

**HTTPS** — TLS/HTTPS still protects properly secured application traffic, but an Evil Twin can expose DNS/domain information, facilitate phishing, and exploit services that don't enforce strong HTTPS protections.

---

## 🚨 06. Rogue Access Points

A rogue AP is **broader** than an Evil Twin. An Evil Twin deliberately impersonates a known SSID; a rogue AP may use any SSID and is dangerous simply because it is unauthorized or creates an uncontrolled network path.

**The major risk — wired bridging**
```text
Uncontrolled RF Environment → Rogue AP → Internal Switch → Protected Network
```
A rogue AP connected to a corporate switch can bridge an uncontrolled wireless environment directly into the protected internal network, potentially bypassing perimeter controls and segmentation.

**How rogue APs appear:**
- **Insider-introduced** — an employee/contractor connects a personal router to an office Ethernet jack for convenience.
- **Deliberately planted** — an attacker with brief physical access places a concealed wireless device on an accessible network port.
- **Forgotten/misconfigured equipment** — previously authorized APs become rogue-like with outdated firmware, legacy protocols, weak configurations, or no-longer-approved deployments.
- **Client-side hotspots** — a laptop or phone hotspot can also create an unauthorized bridge between a protected environment and an uncontrolled wireless network.

---

## 🔍 Detection & Response

**Wireless detection — WIDS/WIPS:** monitor for unknown BSSIDs, unapproved SSIDs, unexpectedly weak wireless configurations, suspicious signal characteristics, duplicate SSIDs, spoofed/duplicate BSSIDs, and abnormal deauthentication patterns. WIDS/WIPS is specifically recommended for identifying duplicate SSIDs/BSSIDs and suspicious deauthentication activity.

**Wired-side detection** can be more reliable than RF monitoring alone: 802.1X, switch-port security, monitoring unexpected MAC addresses, monitoring unexpected DHCP requests, detecting NAT/routing anomalies.

**Physical/RF surveys** — periodic physical walkthroughs and RF surveys can identify concealed devices that other monitoring mechanisms miss.

---

## ⚔️ Evil Twin vs. Rogue AP

| Feature | Evil Twin | Rogue AP |
|---|---|---|
| Category | Specific rogue-AP technique | Broader category |
| SSID | Usually matches legitimate network | May use any name |
| Main objective | Client deception | Unauthorized wireless access/bridging |
| Requires impersonation? | Yes | No |
| Main risk | Credential/data interception | Unauthorized network entry |
| Wired bridge required? | No | Often highly significant |
| Detection focus | Duplicate SSIDs/BSSIDs | RF + wired infrastructure |

The key distinction: an Evil Twin is a specific impersonation technique, while a rogue AP can be dangerous without impersonating anything.

---

## 🗺️ Wireless Attack Landscape

```text
                    Wireless Security
       ┌───────────────────┼────────────────────┐
 Protocol Weakness     Authentication       Trust / Access
       │                   │                    │
   ┌───┼───┐               │              ┌─────┴─────┐
  WEP     WPA           WPS / Reaver    Evil Twin   Rogue AP
   │       │               │              │           │
 RC4/IV  TKIP        PIN weakness      Client trust  Wired bridge
   └───────┴───────────────┴──────────────┴───────────┘
                           │
                         Wifite
                           │
                    Orchestration Layer
```

Conceptual progression across the six PDFs: **protocol weaknesses → authentication weaknesses → automation → client trust → network architecture.**

---

## 🛡️ Defensive Security Recommendations

- **🔐 Use modern Wi-Fi security** — prefer WPA3 where supported; use strong, unique WPA2 passphrases where WPA3 is unavailable; retire WEP completely; avoid legacy WPA/TKIP deployments; keep wireless infrastructure updated (also recommends WIDS/WIPS and current firmware).
- **🚫 Disable WPS when unnecessary** — if it must remain enabled, use robust lockout policies and current firmware.
- **🏢 Use Enterprise authentication** — WPA2/WPA3-Enterprise with proper certificate validation, so clients have a cryptographic basis for rejecting an impostor AP.
- **🛡️ Deploy WIDS/WIPS** — monitor for duplicate SSIDs, duplicate/spoofed BSSIDs, unauthorized APs, abnormal deauthentication, repeated WPS authentication attempts.
- **🔌 Protect wired ports** — 802.1X, port security, device authentication, accurate asset inventories, network segmentation. 802.1X is a direct mitigation against unauthorized AP uplinks reaching internal networks.
- **📐 Segment networks** — limit lateral movement and reduce the impact of a rogue device that goes undetected.
- **🌐 Protect users on untrusted networks** — disable automatic connection to open/low-trust networks, train users to verify network legitimacy, use VPNs for sensitive traffic on untrusted networks, enforce HSTS and modern TLS configurations.

---

## 📖 Recommended Study Order

```text
01. WEP · WPA · WPA2 · WPA3 → 02. Aircrack-ng → 03. Reaver →
04. Wifite → 05. Evil Twin Attacks → 06. Rogue Access Points
```

- Wi-Fi protocols establish the security foundation.
- Aircrack-ng introduces wireless reconnaissance and assessment workflows.
- Reaver introduces WPS as a separate authentication surface.
- Wifite demonstrates workflow automation.
- Evil Twin shifts from cryptographic weaknesses to client trust.
- Rogue APs expand the scope to unauthorized infrastructure and wired-network exposure.

---

## ⚡ Quick Revision Cheat Sheet

| Topic | Remember |
|---|---|
| WEP | RC4 + 24-bit IV → structurally broken |
| WPA | RC4 + TKIP → transitional/deprecated |
| WPA2 | AES-CCMP + 4-way handshake |
| WPA3 | SAE + AES-GCMP + stronger authentication model |
| WPS | Separate feature + weak PIN validation |
| Aircrack-ng | Wireless assessment suite |
| Reaver | WPS-focused assessment |
| Wifite | Automation/orchestration layer |
| Evil Twin | SSID impersonation + client trust |
| Rogue AP | Unauthorized AP; wired bridging can be critical |
| WIDS/WIPS | Wireless detection/prevention |
| 802.1X | Network access control |
| Segmentation | Limits impact of unauthorized access |

---

## 🎯 Key Takeaways

1. Each Wi-Fi security generation addressed specific weaknesses in its predecessor.
2. WEP is completely broken and should not be deployed.
3. WPA/TKIP was an interim technology and is deprecated.
4. WPA2 introduced AES-CCMP and remains widely deployed, but WPA2-Personal is vulnerable to offline password testing when passphrases are weak.
5. WPA3 introduces SAE, which changes the offline password-guessing model.
6. WPS is a separate authentication feature and should be evaluated independently from passphrase strength.
7. Aircrack-ng is a collection of interoperating wireless tools.
8. Reaver focuses on WPS rather than directly attacking the Wi-Fi passphrase.
9. Wifite primarily provides automation rather than a new cryptographic attack.
10. Evil Twin attacks exploit the lack of inherent SSID-to-AP identity binding.
11. Rogue APs are broader than Evil Twins and can be dangerous without impersonation.
12. The most serious rogue-AP risk may be the bridge into the wired internal network.
13. Effective defense requires both RF monitoring and wired access control.

---

## 📁 Folder Structure

```text
13_Wireless_Security
│
├── 01. Wi-Fi Security Evolution (WEP → WPA → WPA2 → WPA3).pdf
├── 02. Aircrack-ng.pdf
├── 03. Reaver.pdf
├── 04. Wifite.pdf
├── 05. Evil Twin Attacks.pdf
├── 06. Rogue Access Points.pdf
└── README.md
```

---

## 🔗 Related Cybersecurity Topics

📡 Wireless Networking · 🔐 Cryptography · 🔑 Authentication · 🌐 Network Security · 🛡️ WPA2/WPA3 Security · 🔎 Network Reconnaissance · 🧪 Penetration Testing · 🏢 Enterprise Network Security · 🔌 802.1X/RADIUS · 📜 PKI & Digital Certificates · 🌐 TLS/HTTPS · 🚨 Wireless IDS/IPS · 🕵️ Security Monitoring · 🧱 Network Segmentation · 👤 Security Awareness

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

## ⚠️ Legal & Ethical Disclaimer

The techniques described in these materials include wireless packet capture, injection, authentication testing, WPS assessment, network impersonation, and rogue-device testing.

Use them only:
- Against wireless equipment you own.
- In a dedicated and isolated security laboratory.
- During an explicitly authorized penetration test.
- In an educational environment where permission has been granted.

Before testing, define the target systems, authorized networks, permitted techniques, testing window, physical-security scope where applicable, and data-handling requirements. Authorization and scope are especially critical for WPS testing, automated multi-target assessment, Evil Twin testing, and physical rogue-AP testing.
