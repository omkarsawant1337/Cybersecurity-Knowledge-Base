# 🔐 Secure Coding

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Level](https://img.shields.io/badge/level-intermediate-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

A structured collection of secure coding principles, common vulnerability classes, and practical secure-development checklists based on the two reference guides included in this repository.

> ⚠️ **Security Note** — use these materials for defensive development, secure code review, authorized testing, and security education.

---

## 📖 Table of Contents

- [Topic Overview](#-topic-overview)
- [Learning Objectives](#-learning-objectives)
- [What Is Secure Coding?](#-what-is-secure-coding)
- [Core Secure Coding Principles](#-core-secure-coding-principles)
- [Secure Coding Across the SDLC](#-secure-coding-across-the-sdlc)
- [Common Vulnerability Classes](#-common-vulnerability-classes)
- [Input Validation Strategy](#-input-validation-strategy)
- [Output Encoding](#-output-encoding)
- [Authentication & Password Management](#-authentication--password-management)
- [Session Management](#-session-management)
- [Access Control](#-access-control)
- [Cryptography & Secrets](#-cryptography--secrets)
- [Error Handling & Logging](#-error-handling--logging)
- [Data & Communication Security](#-data--communication-security)
- [System Configuration](#️-system-configuration)
- [Database Security](#️-database-security)
- [File Management](#-file-management)
- [Memory Management](#-memory-management)
- [General Secure Coding Practices](#-general-secure-coding-practices)
- [Security Tooling in CI/CD](#-security-tooling-in-cicd)
- [Secure Code Review Checklist](#-secure-code-review-checklist)
- [Secure Coding Mental Model](#-secure-coding-mental-model)
- [Recommended Study Order](#-recommended-study-order)
- [Quick Revision Cheat Sheet](#-quick-revision-cheat-sheet)
- [Key Takeaways](#-key-takeaways)
- [Folder Structure](#-folder-structure)
- [Related Cybersecurity Topics](#-related-cybersecurity-topics)
- [Author](#-author)
- [License](#-license)
- [Legal & Ethical Note](#️-legal--ethical-note)

---

## 📚 Topic Overview

| # | Topic | Focus |
|---|---|---|
| 01 | Secure Coding — Principles & Vulnerability Classes | Core principles, SDLC integration, vulnerability root causes and defenses |
| 02 | Secure Coding Practices Checklist | Practical controls for validation, authentication, sessions, access control, crypto, data, databases, files and memory |

---

## 🎯 Learning Objectives

After studying this section, you should be able to:

- Explain secure coding and why it belongs throughout the SDLC.
- Apply least privilege, defense in depth, fail securely, complete mediation, attack-surface minimization, never trust input, and secure-by-default design.
- Integrate threat modeling, secure code review, SAST, DAST, dependency scanning, and secrets scanning into development.
- Recognize common vulnerability classes and their root causes.
- Apply server-side input validation and contextual output encoding.
- Build secure authentication and session-management mechanisms.
- Enforce server-side authorization on every protected request.
- Handle cryptography, secrets, dependencies, logging, and errors securely.
- Protect databases, file uploads, memory, and server-side requests.

---

## 🧭 What Is Secure Coding?

Secure coding means writing software that anticipates and defends against misuse, treating inputs, dependencies, and trust boundaries as potentially adversarial. It is a continuous practice across:

```text
Design → Coding → Testing → Deployment → Maintenance
```

Its goal is to prevent vulnerabilities from being introduced rather than relying only on later testing and patching. Security testing, code review, and threat modeling complement secure coding but do not replace it.

```text
Every Input / Dependency / Trust Boundary
                    ↓
             Potentially Adversarial
                    ↓
        Validate • Restrict • Protect
                    ↓
             Fail Securely
```

---

## 🏛️ Core Secure Coding Principles

- **Least Privilege** — every component, process, account, and piece of code should have only the access it needs. Limiting privilege reduces the blast radius of compromise.
- **Defense in Depth** — never rely on one security control. Layer input validation, output encoding, authorization, least privilege, and monitoring so bypassing one control does not automatically cause full compromise.
- **Fail Securely** — unexpected errors and exceptions should result in a secure state: deny access, refuse the operation, or halt safely rather than fail open.
- **Complete Mediation** — check authorization against the current security state for every access to every protected resource. Don't depend on stale or one-time checks.
- **Minimize Attack Surface** — disable unused features, close unnecessary ports, remove production debug endpoints, and question whether exposed functionality genuinely needs to be reachable.
- **Never Trust User Input** — treat form fields, URL parameters, headers, uploads, API payloads, cookies, and user-originated stored data as untrusted until validated.
- **Secure by Default** — default configurations should be reasonably secure. Weakening security should require a deliberate decision, rather than hardening being required after installation.

---

## 🔄 Secure Coding Across the SDLC

| Phase | Secure Coding Activity |
|---|---|
| Requirements & Design | Threat modeling, trust boundaries, data flows, attacker goals, security requirements |
| Implementation | Secure principles, vetted libraries/frameworks, secure language-specific practices |
| Code Review | Review for security issues and known vulnerability classes |
| Testing | SAST, DAST, dependency/software-composition analysis |
| Deployment | Hardened configuration, secure defaults, reduced exposed surface |
| Maintenance | Patching, dependency updates, monitoring, incident-response readiness |

Threat modeling is placed before implementation, with security tooling running throughout testing/CI/CD.

---

## 🐛 Common Vulnerability Classes

### Injection Flaws
**Root cause:** untrusted input is concatenated into a query, command, or interpreter context, allowing the input to alter the operation's structure.
**Defense:** use parameterized/prepared statements and safe structured APIs rather than manually constructed command strings.
```text
Untrusted Input → Parameterized / Structured API → Data stays data
```

### Cross-Site Scripting (XSS)
**Root cause:** untrusted input is rendered without appropriate contextual encoding.
**Defense:** context-appropriate output encoding plus a properly configured Content Security Policy (CSP) as defense in depth. HTML, JavaScript, and URL contexts require different encoding.

### Broken Authentication & Session Management
**Root causes:** weak password policies, predictable/poorly-protected session identifiers, missing expiration, inadequate credential protection.
**Defense:** strong salted password hashing (Argon2/bcrypt), cryptographically random session tokens, proper expiration/invalidation, session regeneration, MFA where appropriate.

### Broken Access Control
**Root cause:** missing/inconsistent authorization or reliance on client-controlled identifiers.
**Defense:** enforce authorization server-side, on every request, for every protected resource. Never treat hidden URLs or UI elements as access control.

### Security Misconfiguration
**Common causes:** default accounts, unnecessary services, verbose errors, missing security headers, unpatched software.
**Defense:** hardened minimal configurations, regular audits, automated configuration management, generic production errors.

### Cryptographic Failures
**Causes:** weak/deprecated algorithms, missing encryption, poor key management, custom cryptography.
**Defense:** use current vetted algorithms and established libraries; never implement cryptographic primitives from scratch; use TLS for data in transit and dedicated password-hashing functions for credentials.

### Insecure Deserialization
Untrusted data may be able to reconstruct arbitrary objects or trigger code as a side effect.
**Defense:** avoid unsafe deserialization; use formats/libraries that don't permit arbitrary object instantiation and restrict permitted types.

### Components with Known Vulnerabilities
Untracked third-party dependencies can introduce vulnerabilities into otherwise secure application code.
**Defense:** maintain an up-to-date SBOM, automate dependency scanning, establish a process for evaluating and applying patches.

### Insufficient Logging & Monitoring
Security events that aren't logged or monitored can allow successful attacks to remain undetected.
**Defense:** log relevant authentication, authorization, validation, tampering, and administrative events; protect log integrity; integrate logs with active monitoring/alerting.

### Server-Side Request Forgery (SSRF)
**Root cause:** a server fetches attacker-influenced URLs/resources without sufficiently restricting destinations.
**Defense:** strictly validate/allowlist permitted hosts and schemes; use network segmentation so successful SSRF has limited internal reach.

---

## ✅ Input Validation Strategy

Emphasize **server-side validation**, even when client-side validation is also present.

| Approach | Meaning | Guidance |
|---|---|---|
| Allowlisting | Define exactly what valid input looks like | ✅ Preferred |
| Denylisting | Block known-bad patterns | ⚠️ Weaker; variants/obfuscation can bypass it |
| Sanitization | Modify dangerous content | 🛡️ Defense in depth, not the sole control |

**Practical validation checklist**
- Validate client-provided data on the server.
- Identify and classify trusted/untrusted sources.
- Validate data from databases, streams, and redirects when user-influenced.
- Centralize validation routines.
- Normalize/canonicalize where needed.
- Validate type, length, range, and format.
- Reject invalid data rather than relying on sanitization alone.

Client-side validation is a usability convenience, **not** a security control.

---

## 🔤 Output Encoding

Input validation and output encoding are different controls. Output encoding transforms data at the point where it enters a context so that it is interpreted as data rather than executable syntax.

**Relevant contexts:** HTML, JavaScript, URL, CSS, SQL, LDAP, shell commands.

The correct encoding depends on the destination context — there is no single generic encoding/sanitization function suitable for every context.

---

## 🔑 Authentication & Password Management

- Authenticate all non-public resources.
- Centralize authentication.
- Hash passwords with strong salted one-way algorithms such as Argon2 or bcrypt.
- Avoid revealing which authentication field failed.
- Use secure credential transmission.
- Enforce appropriate password policies.
- Apply account lockout/rate controls where appropriate.
- Require MFA for sensitive/administrative accounts.
- Protect password reset with verification and temporary tokens.

---

## 🎟️ Session Management

Secure sessions reduce session hijacking and fixation risks.

- Use secure framework session-management components.
- Generate session IDs server-side with strong randomness.
- Regenerate session IDs after login or privilege elevation.
- Never expose session identifiers in URLs or logs.
- Use `HttpOnly` and `Secure` cookie attributes.
- Configure appropriate session timeouts.
- Terminate sessions securely on logout.
- Apply stronger controls to sensitive applications.

---

## 🛡️ Access Control

```text
Request → Authenticate → Authorize → Allowed?
                                        ├── Yes → Continue
                                        └── No  → Deny
```

- Centralize authorization checks.
- Enforce authorization on every request, including APIs and server-side scripts.
- Deny by default.
- Make authorization decisions using server-side state.
- Protect URLs, files, functions, and services.
- Restrict direct object references.
- Revalidate privileges periodically.
- Apply rate limits to automated abuse.

---

## 🔒 Cryptography & Secrets

**Cryptography**
- Use strong, vetted algorithms and established libraries.
- Protect cryptographic keys with strict access controls.
- Use strong cryptographic random-number generators.
- Fail securely if crypto operations malfunction.
- Use TLS for sensitive data in transit.
- Use dedicated password-hashing functions for credentials.
- **Never implement cryptographic primitives from scratch.**

**Secrets management** — secrets include API keys, database credentials, encryption keys, and tokens.

```text
Never: API_KEY = "hard-coded-secret"
```

Do not hard-code secrets or commit them to version control. Prefer dedicated secret-management systems, deployment-time injection, or cloud secret managers. Rotate secrets periodically and immediately after suspected compromise; apply least privilege to secret access.

---

## 📋 Error Handling & Logging

Detailed stack traces and debug output can reveal internal paths, database structures, library versions, and other reconnaissance information.

**Recommended pattern**
```text
User   → Generic Safe Error
Server → Detailed Protected Diagnostic Log
```

Log security-relevant events while restricting access to logs, protecting their integrity, and avoiding unnecessary sensitive information.

---

## 🔐 Data & Communication Security

**Data protection**
- Apply least privilege to sensitive data.
- Encrypt highly sensitive information where appropriate.
- Avoid sensitive data in HTTP GET parameters.
- Disable browser caching for sensitive pages.
- Securely delete temporary sensitive data when no longer required.

**Communication security**
- Use TLS for sensitive communications.
- Reject invalid certificates.
- Never downgrade to insecure protocols.
- Maintain consistent TLS settings.
- Filter sensitive data from HTTP referer headers.

---

## ⚙️ System Configuration

- Patch systems promptly.
- Remove unused files, features, and services.
- Remove production test/debug code.
- Disable directory listings and unnecessary HTTP methods.
- Remove unnecessary version information from response headers.
- Separate development, staging, and production.
- Use configuration stores with auditing.

---

## 🗄️ Database Security

```text
User Input → Validation → Parameterized Query → Database
```

- Use strongly typed parameterized queries.
- Validate/encode input appropriately.
- Apply least privilege to database accounts.
- Store connection credentials securely.
- Use suitable database abstraction mechanisms.
- Remove default accounts and sample schemas.

---

## 📁 File Management

Unsafe file handling can lead to arbitrary execution, traversal, and malicious-upload vulnerabilities.

**Upload checklist**
- Authenticate users before uploads.
- Validate file types using file headers, not only extensions.
- Store uploads outside the webroot.
- Disable script execution in upload directories.
- Never expose absolute filesystem paths.
- Scan uploads for malware.

---

## 🧠 Memory Management

Memory-safety issues can cause crashes, information disclosure, or remote code execution.

- Check buffer boundaries.
- Restrict/truncate inputs appropriately.
- Avoid unsafe functions with known vulnerabilities.
- Use non-executable memory protections where supported.
- Free memory correctly.
- Clear sensitive data before release where appropriate.

---

## 🧰 General Secure Coding Practices

- Prefer managed code and built-in APIs for OS operations.
- Avoid dynamic code execution from user input.
- Validate and restrict third-party libraries.
- Prevent race conditions using appropriate synchronization.
- Initialize variables before use.
- Elevate privileges only when necessary and drop them immediately afterward.
- Verify code integrity using checksums/hashes.
- Deliver secure updates over encrypted channels.

---

## 🔧 Security Tooling in CI/CD

```text
Developer → Commit → CI/CD Pipeline
                        ├── SAST
                        ├── DAST
                        ├── Dependency Scanning
                        └── Secrets Scanning
                              ↓
                        Security Review → Deployment
```

- **SAST** (Static Application Security Testing) — analyzes source code for risky patterns.
- **DAST** (Dynamic Application Security Testing) — probes a running application.
- **Dependency Scanning** — identifies known vulnerabilities in third-party components.
- **Secrets Scanning** — detects accidentally committed credentials, tokens, and similar secrets.

Recommended to integrate these automated tools into CI/CD, alongside threat modeling, security-focused code review, and developer training.

---

## ✅ Secure Code Review Checklist

**🔐 Authentication:** strong password hashing · MFA for sensitive/admin accounts · secure password reset · authentication on protected resources · generic authentication errors

**🎟️ Sessions:** cryptographically random session IDs · session ID regeneration after login/privilege elevation · HttpOnly cookies · secure cookies · appropriate timeouts · secure logout/invalidation

**🛡️ Authorization:** server-side checks · authorization on every protected request · deny-by-default behavior · no reliance on hidden UI/URLs · direct object references protected

**🧪 Input & Output:** server-side validation · allowlisting where appropriate · canonicalization where necessary · contextual output encoding · no unsafe string concatenation into interpreters

**🗄️ Database:** parameterized queries · least-privilege database accounts · secure connection secrets · no default/sample accounts

**📁 Files:** authenticated uploads · file-header/type validation · uploads outside webroot · script execution disabled in upload directories · malware scanning

**🔑 Secrets & Crypto:** no hard-coded secrets · secrets manager used · vetted cryptography · strong randomness · key access restricted · TLS for sensitive traffic

**⚙️ Configuration:** secure defaults · debug disabled in production · unused services removed · security patches applied · environments separated

**🚨 Logging:** authentication failures logged · authorization failures logged · validation failures logged · administrative actions logged · log integrity protected · monitoring/alerting configured · sensitive data excluded where unnecessary

---

## 🧩 Secure Coding Mental Model

```text
                 SECURE CODING
       ┌───────────────┼────────────────┐
     DESIGN          CODE             OPERATE
 Threat Model     Validate Input     Monitor
 Trust Boundaries Encode Output      Patch
 Security Req.    AuthN/AuthZ        Respond
                  Safe APIs           Harden
       └───────────────┼────────────────┘
                       ↓
              DEFENSE IN DEPTH
                       ↓
             SECURE APPLICATION
```

---

## 📖 Recommended Study Order

```text
01. Secure Coding Overview → 02. Core Principles → 03. Secure SDLC →
04. Vulnerability Classes → 05. Input Validation → 06. Output Encoding →
07. Authentication & Sessions → 08. Access Control → 09. Cryptography & Secrets →
10. Error Handling & Logging → 11. Data / Communication / Configuration →
12. Database / File / Memory Security → 13. CI/CD Security Tooling →
14. Secure Code Review Checklist
```

---

## ⚡ Quick Revision Cheat Sheet

| Concept | Key Point |
|---|---|
| Least Privilege | Give only required permissions |
| Defense in Depth | Use multiple independent controls |
| Fail Securely | Errors should default to a secure state |
| Complete Mediation | Check authorization every time |
| Attack Surface | Minimize exposed functionality |
| Never Trust Input | Treat external/user-influenced data as untrusted |
| Secure by Default | Secure configuration should be the default |
| Injection | Use parameterized/structured APIs |
| XSS | Use contextual output encoding |
| Authentication | Strong hashing, secure sessions, MFA where appropriate |
| Access Control | Server-side, every protected request |
| Cryptography | Use vetted algorithms/libraries; don't invent crypto |
| Deserialization | Avoid unsafe reconstruction from untrusted data |
| Dependencies | Track, scan and patch components |
| Logging | Log security events and monitor them |
| SSRF | Strictly restrict server-side destinations |
| Input Validation | Prefer allowlisting |
| Secrets | Never hard-code or commit secrets |
| Errors | Generic externally, detailed internally |
| Files | Validate and isolate uploads |
| Memory | Respect boundaries and avoid unsafe operations |
| CI/CD | Integrate SAST, DAST, dependency and secrets scanning |

---

## 🎯 Key Takeaways

1. Secure coding is a continuous SDLC discipline, not a final testing phase.
2. Treat inputs, dependencies, and trust boundaries as potentially adversarial.
3. Least privilege reduces blast radius.
4. Defense in depth prevents one failed control from becoming full compromise.
5. Fail securely and deny access when security checks cannot be completed safely.
6. Apply complete mediation to protected resources.
7. Minimize unnecessary attack surface.
8. Prefer allowlisting for input validation.
9. Client-side validation is not a security boundary.
10. Output encoding must match the destination context.
11. Use strong password hashing and secure session management.
12. Enforce authorization server-side on every protected request.
13. Never implement cryptographic primitives yourself.
14. Never hard-code secrets or commit them to source control.
15. Track and patch third-party dependencies.
16. Protect and monitor security logs.
17. SSRF defenses require both application-level destination restrictions and network segmentation.
18. Secure file handling requires validation, isolation, and malware scanning.
19. CI/CD security tooling should complement secure coding, threat modeling, review, and developer training.

---

## 📁 Folder Structure

```text
14_Secure_Coding
│
├── 01. Secure Coding - Principles & Vulnerability Classes.pdf
├── 02. Secure Coding Practices Checklist (based on OWASP guidance).pdf
└── README.md
```

---

## 🔗 Related Cybersecurity Topics

🔐 Application Security · 🛡️ OWASP · 🧪 Secure SDLC · 🔎 Vulnerability Management · 🧑‍💻 Secure Code Review · 📦 Software Supply Chain Security · 🔑 Authentication & Authorization · 🗄️ Database Security · 🌐 Web Application Security · 🔒 Cryptography · 🚨 Security Monitoring · ⚙️ CI/CD Security · 🧪 SAST/DAST · 🔑 Secrets Management

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

## ⚠️ Legal & Ethical Note

This repository is intended for defensive security, secure software development, authorized testing, education, and code review. The source material identifies these practices as applicable to secure SDLC, code reviews, penetration-testing remediation, developer training, and internal compliance audits.
