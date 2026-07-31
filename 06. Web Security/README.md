# 🌐 Web Security

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Level](https://img.shields.io/badge/level-beginner--to--intermediate-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

> Learn the most critical web application security vulnerabilities, modern attack techniques, and defensive best practices used in real-world penetration testing and bug bounty hunting. This section covers the **OWASP Top 10 (2025)**, injection attacks, authentication flaws, session management, access control issues, and advanced web exploitation techniques with practical examples.

Topics roughly fall into five groups: **OWASP & fundamentals**, **injection vulnerabilities**, **access control & client-side vulnerabilities**, **server-side & file-based vulnerabilities**, and **advanced web exploitation** — see the Table of Contents below for the full breakdown.

---

## 📖 Table of Contents

- [Recommended Learning Path](#-recommended-learning-path)
- [Topics Covered](#-topics-covered)
  - [OWASP Top 10 (2025)](#-owasp-top-10-2025)
  - [SQL Injection (SQLi)](#-sql-injection-sqli)
  - [Server-Side Request Forgery (SSRF)](#-server-side-request-forgery-ssrf)
  - [Cross-Origin Resource Sharing (CORS)](#-cross-origin-resource-sharing-cors)
  - [Cross-Site Request Forgery (CSRF)](#-cross-site-request-forgery-csrf)
  - [Cross-Site Scripting (XSS)](#-cross-site-scripting-xss)
  - [Clickjacking (UI Redressing)](#-clickjacking-ui-redressing)
  - [File Path Traversal](#-file-path-traversal)
  - [File Upload Vulnerabilities](#-file-upload-vulnerabilities)
  - [WebSockets Security](#-websockets-security)
  - [Insecure Direct Object Reference (IDOR)](#-insecure-direct-object-reference-idor)
  - [XML External Entity (XXE)](#-xml-external-entity-xxe)
  - [Server-Side Template Injection (SSTI)](#-server-side-template-injection-ssti)
  - [OS Command Injection](#-os-command-injection)
  - [NoSQL Injection](#-nosql-injection)
  - [Open Redirect](#-open-redirect)
  - [Authentication Vulnerabilities](#-authentication-vulnerabilities)
  - [Sessions & Session Management](#-sessions--session-management)
  - [Cookies](#-cookies)
  - [JWT Attacks](#-jwt-attacks)
  - [OAuth 2.0 Authentication Vulnerabilities](#-oauth-20-authentication-vulnerabilities)
  - [Race Conditions](#-race-conditions)
  - [Business Logic Vulnerabilities](#-business-logic-vulnerabilities)
  - [Web Cache Poisoning & Deception](#-web-cache-poisoning--web-cache-deception)
  - [HTTP Request Smuggling & Response Splitting](#-http-request-smuggling--http-response-splitting)
  - [CRLF Injection](#-crlf-injection)
  - [Prototype Pollution](#-prototype-pollution)
- [Skills You'll Learn](#-skills-youll-learn)
- [Folder Structure](#-folder-structure)
- [Why Learn Web Security?](#-why-learn-web-security)
- [Author](#-author)
- [License](#-license)

---

## 🎯 Recommended Learning Path

Study the topics in the following order (matches the numbering used in the folder):

1. OWASP Top 10 (2025)
2. SQL Injection (SQLi)
3. Server-Side Request Forgery (SSRF)
4. Cross-Origin Resource Sharing (CORS)
5. Cross-Site Request Forgery (CSRF)
6. Cross-Site Scripting (XSS)
7. Clickjacking (UI Redressing)
8. File Path Traversal (Directory Traversal)
9. File Upload Vulnerabilities
10. WebSockets Security
11. IDOR (Insecure Direct Object Reference)
12. XXE (XML External Entity) Injection
13. SSTI (Server-Side Template Injection)
14. OS Command Injection
15. NoSQL Injection
16. Open Redirect
17. Authentication Vulnerabilities
18. Sessions & Session Management
19. Cookies
20. JWT Attacks
21. OAuth 2.0 Authentication Vulnerabilities
22. Race Conditions
23. Business Logic Vulnerabilities
24. Web Cache Poisoning & Web Cache Deception
25. HTTP Request Smuggling & HTTP Response Splitting
26. CRLF Injection
27. Prototype Pollution

---

## 📂 Topics Covered

### 🏆 OWASP Top 10 (2025)

Understand the latest OWASP Top 10 web application security risks and their impact on modern applications. The 2025 edition (finalized January 2026) is the first major revision since 2021.

Topics include:

- A01 Broken Access Control *(now absorbs SSRF)*
- A02 Security Misconfiguration
- A03 Software Supply Chain Failures *(new for 2025)*
- A04 Cryptographic Failures
- A05 Injection
- A06 Insecure Design
- A07 Authentication Failures
- A08 Software & Data Integrity Failures
- A09 Security Logging & Alerting Failures *(renamed from "Monitoring" to "Alerting" in 2025)*
- A10 Mishandling of Exceptional Conditions *(new for 2025)*
- Changes from OWASP Top 10 2021 & Mapping to CWE/CVE
- Mitigation Strategies

### 💉 SQL Injection (SQLi)

Topics include:

- Authentication Bypass & UNION Attacks
- Error-Based, Boolean-Based Blind & Time-Based Blind SQLi
- Out-of-Band SQLi & Database Enumeration
- WAF Bypass Techniques
- Prevention & Mitigation

### 🌐 Server-Side Request Forgery (SSRF)

Topics include:

- SSRF Fundamentals & Blind SSRF
- Cloud Metadata Exploitation
- Internal Network Scanning & URL Parsing Bypasses
- SSRF Chains & Mitigation Techniques

### 🌍 Cross-Origin Resource Sharing (CORS)

Topics include:

- Same-Origin Policy & `Access-Control-Allow-Origin`
- Credentialed Requests & Origin Reflection
- Wildcard Origins & Null Origin
- Common Misconfigurations & Secure Configuration

### 🔄 Cross-Site Request Forgery (CSRF)

Topics include:

- CSRF Workflow — Login CSRF, JSON API CSRF
- CSRF Tokens & SameSite Cookies
- Origin Validation & Prevention Techniques

### 💻 Cross-Site Scripting (XSS)

Topics include:

- Reflected, Stored & DOM XSS
- XSS Payloads, Cookie Theft, Session Hijacking
- CSP & Prevention

### 🖱 Clickjacking (UI Redressing)

Topics include:

- UI Redressing & Iframe Attacks
- Frame Busting
- X-Frame-Options & CSP Frame-Ancestors
- Mitigation

### 📂 File Path Traversal

Topics include:

- Directory Traversal & Path Normalization
- Double Encoding & Null Byte Injection
- File Disclosure & Secure File Access

### 📤 File Upload Vulnerabilities

Topics include:

- Web Shell Uploads & MIME Type Bypass
- Double Extensions & File Validation
- RCE via File Upload & Prevention

### 🔌 WebSockets Security

Topics include:

- WebSocket Handshake & Burp Suite Testing
- Authentication & Authorization
- Cross-Site WebSocket Hijacking
- Secure Implementation

### 🔑 Insecure Direct Object Reference (IDOR)

Topics include:

- Horizontal & Vertical IDOR
- API IDOR & Object Enumeration
- Access Control Validation & Prevention

### 📄 XML External Entity (XXE)

Topics include:

- XML Parsing & External Entities
- Blind XXE & Out-of-Band XXE
- Local File Disclosure & SSRF via XXE
- Secure XML Parsing

### 🧩 Server-Side Template Injection (SSTI)

Topics include:

- Template Engines — Detection & Fingerprinting
- Sandbox Escape & Remote Code Execution
- Secure Template Rendering

### 💻 OS Command Injection

Topics include:

- Shell Injection & Blind Command Injection
- Reverse Shell Concepts & OS Enumeration
- Input Validation & Mitigation

### 🗄 NoSQL Injection

Topics include:

- MongoDB Injection & Operator Injection
- Authentication Bypass & Data Extraction
- Secure Queries

### ↩ Open Redirect

Topics include:

- Redirect Validation & Phishing Abuse
- OAuth Abuse & SSRF Filter Bypass
- Secure Redirect Design

### 🔐 Authentication Vulnerabilities

Topics include:

- Broken Authentication & Brute Force
- Username Enumeration & Password Reset Flaws
- MFA Weaknesses & Login Logic Flaws
- Secure Authentication

### 🍪 Sessions & Session Management

Topics include:

- Session IDs & Session Cookies
- Session Fixation & Session Hijacking
- Secure Cookie Attributes & Session Timeout
- Best Practices

### 🍪 Cookies

Topics include:

- Cookie Fundamentals & `Set-Cookie` Header
- Cookie Attributes — Domain & Path Scoping
- Session vs. Persistent Cookies
- SameSite Deep Dive, Secure, HttpOnly, Cookie Prefixes
- Third-Party Cookies & Browser Storage Comparison
- Cookie Security Best Practices

### 🔑 JWT Attacks

Topics include:

- JWT Structure — JWS vs. JWE, JWT Claims
- Signature Verification & `alg:none` Attack
- Weak Secret Brute Force & Header Parameter Injection
- JWK Injection, `kid` Injection, `jwk`/`jku` Abuse
- Algorithm Confusion & Secure JWT Validation

### 🔓 OAuth 2.0 Authentication Vulnerabilities

Topics include:

- OAuth Architecture — Authorization Code Flow & Implicit Flow
- OAuth Discovery & State Parameter
- Redirect URI Validation & Authorization Code Leakage
- Token Leakage & OAuth Login CSRF
- PKCE, OpenID Connect, Secure OAuth Implementation

### ⚡ Race Conditions

Topics include:

- TOCTOU & Limit Overrun
- Parallel Request Attacks & Hidden Multi-Step Races
- Multi-Endpoint Race Conditions & Single-Packet Attack
- Burp Repeater & Turbo Intruder
- Detection Methodology & Mitigation Strategies

### 🧠 Business Logic Vulnerabilities

Topics include:

- Insecure Design & Workflow Manipulation
- Client-Side Trust & Price Manipulation
- Coupon Abuse & Negative Value Exploitation
- Multi-Step Workflow Abuse & Authentication Logic Flaws
- Sequence Bypass & Server-Side Validation
- Business Logic Testing

### ⚙ Web Cache Poisoning & Web Cache Deception

Topics include:

- Cache Architecture & Cache Keys
- Unkeyed Inputs & Cache Poisoning/Deception
- Param Miner & Header Injection
- Fat GET Requests & Cache Key Manipulation
- Cache Probing & CPDoS
- Mitigation Techniques

### 📡 HTTP Request Smuggling & HTTP Response Splitting

Topics include:

- Front-End / Back-End Desynchronization
- CL.TE, TE.CL, TE.TE
- Timing-Based Detection & Response Queue Poisoning
- Cache Poisoning & WAF Bypass
- HTTP Response Splitting & Mitigation Strategies

### 📝 CRLF Injection

Topics include:

- CRLF Basics & HTTP Header Injection
- HTTP Response Splitting & Cookie Injection
- Cache Poisoning & Log Forging
- SMTP Header Injection & Protocol Injection
- URL Encoding Bypasses & Secure Header Construction

### 🧬 Prototype Pollution

Topics include:

- JavaScript Prototype Chain & `Object.prototype`
- Recursive Merge, `__proto__`, `constructor.prototype`
- Source & Gadget Discovery, DOM Invader
- Client-Side & Server-Side Prototype Pollution
- DOM XSS via Prototype Pollution
- Node.js Exploitation & Mitigation Strategies

---

## 🛠 Skills You'll Learn

- OWASP Top 10 (2025) & Web Application Penetration Testing
- Injection Testing — SQLi, NoSQLi, SSTI, XXE, Command Injection
- Authentication & Authorization Testing
- Session & Cookie Security
- API & WebSocket Security Testing
- JWT & OAuth Security Assessment
- Business Logic & Race Condition Testing
- Web Cache Attacks & HTTP Request Smuggling
- CRLF Injection & Prototype Pollution
- Burp Suite Professional Workflow
- Secure Coding Practices & Professional Vulnerability Reporting

---

## 📁 Folder Structure

```text
06_Web_Security
│
├── 01. OWASP Top 10 2025.pdf
├── 02. SQL Injection (SQLi).pdf
├── 03. Server-Side Request Forgery (SSRF).pdf
├── 04. Cross Origin Resource Sharing (CORS).pdf
├── 05. Cross-Site Request Forgery (CSRF).pdf
├── 06. Cross-Site Scripting (XSS).pdf
├── 07. Clickjacking (UI Redressing).pdf
├── 08. File Path Traversal (Directory Traversal).pdf
├── 09. File Upload Vulnerabilities.pdf
├── 10. WebSockets Security.pdf
├── 11. IDOR (Insecure Direct Object Reference).pdf
├── 12. XXE (XML External Entity) Injection.pdf
├── 13. SSTI (Server-Side Template Injection).pdf
├── 14. OS Command Injection.pdf
├── 15. NoSQL Injection.pdf
├── 16. Open Redirect.pdf
├── 17. Authentication Vulnerabilities.pdf
├── 18. Sessions & Session Management.pdf
├── 19. Cookies.pdf
├── 20. JWT Attacks.pdf
├── 21. OAuth 2.0 Authentication Vulnerabilities.pdf
├── 22. Race Conditions.pdf
├── 23. Business Logic Vulnerabilities.pdf
├── 24. Web Cache Poisoning & Web Cache Deception.pdf
├── 25. HTTP Request Smuggling & HTTP Response Splitting.pdf
├── 26. CRLF Injection.pdf
├── 27. Prototype Pollution.pdf
└── README.md
```

---

## 🚀 Why Learn Web Security?

Web applications power nearly every modern organization, making web security one of the most valuable skills in cybersecurity. Mastering these topics enables you to identify vulnerabilities, understand attack chains, assess secure implementations, and recommend effective mitigations.

By completing this section, you'll be able to:

- Perform professional Web Application Penetration Testing
- Understand and apply the OWASP Top 10 (2025)
- Assess authentication, authorization, and session security
- Test REST APIs and WebSocket applications
- Analyze modern attack chains and advanced exploitation techniques
- Perform manual testing beyond automated scanners
- Write professional vulnerability reports
- Prepare for certifications such as eJPT, PNPT, CEH, OSCP, CRTP, BSCP, and bug bounty programs

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
