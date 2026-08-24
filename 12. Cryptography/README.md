# 🔐 Cryptography

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Level](https://img.shields.io/badge/level-intermediate-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

A structured collection of cryptography and applied cryptographic security concepts covering encryption, hashing, password protection, digital signatures, PKI, digital certificates, and TLS/HTTPS.

This section of the **Cybersecurity-Knowledge-Base** progresses from cryptography fundamentals to the algorithms and trust infrastructure used to protect modern systems and network communications.

---

## 📖 Table of Contents

- [Topic Overview](#-topic-overview)
- [Learning Objectives](#-learning-objectives)
- [01. Cryptography Fundamentals](#-01-cryptography-fundamentals)
- [02. Symmetric vs. Asymmetric Cryptography](#-02-symmetric-vs-asymmetric-cryptography)
- [03. AES — Advanced Encryption Standard](#-03-aes--advanced-encryption-standard)
- [04. RSA — Rivest–Shamir–Adleman](#-04-rsa--rivestshamiradleman)
- [05. ECC — Elliptic Curve Cryptography](#-05-ecc--elliptic-curve-cryptography)
- [06. Cryptographic Hashing & SHA](#-06-cryptographic-hashing--sha)
- [07. MD5](#-07-md5--message-digest-algorithm-5)
- [08–10. Password Hashing (bcrypt, Argon2, PBKDF2)](#-0810-password-hashing)
- [11. Digital Signatures](#️-11-digital-signatures)
- [12. PKI — Public Key Infrastructure](#-12-pki--public-key-infrastructure)
- [13. Digital Certificates](#-13-digital-certificates)
- [14. TLS & HTTPS](#-14-tls--https)
- [How the Topics Connect](#-how-the-topics-connect)
- [Encryption vs. Hashing vs. Password Hashing vs. Signatures](#-encryption-vs-hashing-vs-password-hashing-vs-signatures)
- [Algorithm Status Cheat Sheet](#-algorithm-status-cheat-sheet)
- [Recommended Study Order](#-recommended-study-order)
- [Quick Revision Cheat Sheet](#-quick-revision-cheat-sheet)
- [Folder Structure](#-folder-structure)
- [Related Cybersecurity Topics](#-related-cybersecurity-topics)
- [Author](#-author)
- [License](#-license)
- [Disclaimer](#️-disclaimer)

---

## 📚 Topic Overview

| # | Topic | Focus |
|---|---|---|
| 01 | Cryptography | Fundamentals, terminology, encryption models, symmetric/asymmetric cryptography, hashing, security goals |
| 02 | AES (Advanced Encryption Standard) | Symmetric block encryption, AES structure, rounds and security |
| 03 | RSA (Rivest–Shamir–Adleman) | Asymmetric cryptography, key generation, encryption and signatures |
| 04 | ECC (Elliptic Curve Cryptography) | Elliptic-curve public-key cryptography, ECDH, ECDSA/EdDSA, shorter keys |
| 05 | SHA (Secure Hash Algorithm) | Cryptographic hashing, SHA-1, SHA-2, SHA-3, hash properties |
| 06 | MD5 (Message Digest Algorithm 5) | MD5 design, collision attacks and weaknesses |
| 07 | bcrypt | Password hashing, salting, tunable computational cost |
| 08 | Argon2 | Memory-hard password hashing, configurable resources |
| 09 | PBKDF2 | Password-based key derivation, HMAC, salts, iterations |
| 10 | Digital Signatures | Authentication, integrity, non-repudiation |
| 11 | PKI (Public Key Infrastructure) | Certificate authorities, trust chains, identity binding |
| 12 | Digital Certificates | X.509 certificates, fields, validation, trust |
| 13 | TLS & HTTPS | Secure transport, TLS 1.2/1.3 handshake, certificate-based authentication |

---

## 🎯 Learning Objectives

After studying this section, you should be able to:

- Explain the purpose of cryptography in cybersecurity.
- Distinguish plaintext, ciphertext, cipher, key, encryption, decryption, and cryptanalysis.
- Explain symmetric vs. asymmetric cryptography.
- Understand encryption vs. hashing.
- Describe AES and its internal round structure.
- Understand RSA key generation and public/private key usage.
- Explain why ECC can provide comparable security with shorter keys.
- Understand pre-image, second pre-image, and collision resistance.
- Explain why MD5 and SHA-1 are unsuitable for security-sensitive purposes.
- Compare bcrypt, Argon2, and PBKDF2 for password protection.
- Explain digital signatures and their security guarantees.
- Understand how PKI binds public keys to verified identities.
- Understand X.509 digital certificates.
- Explain how TLS combines certificates, asymmetric cryptography, signatures, and symmetric encryption.

---

## 🔑 01. Cryptography Fundamentals

Cryptography is the science and practice of securing information through mathematical algorithms to prevent unauthorized access, interpretation, or modification — transforming an intelligible message into an unintelligible form and restoring it to its original form. Key security goals: **confidentiality, integrity, authentication, and non-repudiation.**

**Basic terminology**

| Term | Meaning |
|---|---|
| Plaintext | Original, intelligible message |
| Ciphertext | Transformed/coded message |
| Cipher | Algorithm used to transform plaintext into ciphertext |
| Key | Critical information used by the cipher |
| Encrypt / Encipher | Convert plaintext into ciphertext |
| Decrypt / Decipher | Convert ciphertext back into plaintext |
| Cryptography | Study of encryption principles and methods |
| Cryptanalysis | Study of recovering plaintext or breaking cryptographic protection without the key |

The general encryption model takes plaintext and a key as inputs to an encryption algorithm and produces ciphertext.

---

## 🔐 02. Symmetric vs. Asymmetric Cryptography

### Symmetric Cryptography

Uses the same secret key for encryption and decryption.

```text
Secret Key
    │
Plaintext → Encryption → Ciphertext → Decryption → Plaintext
                                            ↑
                                       Secret Key
```

**AES** is the main symmetric encryption algorithm covered in this collection.

### Asymmetric Cryptography

Uses mathematically related public and private keys. **RSA** uses the public key for encryption/signature verification and the private key for decryption/signature creation. **ECC** is another public-key approach based on elliptic curves over finite fields.

---

## 🧱 03. AES — Advanced Encryption Standard

AES is a symmetric-key block cipher standardized by NIST in 2001 as the successor to DES, which was replaced as its 56-bit key became increasingly vulnerable to brute-force attacks.

**AES at a glance**

| Property | Detail |
|---|---|
| Type | Symmetric-key block cipher |
| Block size | 128 bits |
| Key sizes | 128, 192, 256 bits |
| Structure | Substitution-Permutation Network (SPN) |
| Standardized | NIST, 2001 |
| Based on | Rijndael |

**AES rounds**

| Key Size | Rounds |
|---|---|
| 128-bit | 10 |
| 192-bit | 12 |
| 256-bit | 14 |

AES operates on a 4×4 byte state. Round transformations: **SubBytes** (nonlinear byte substitution), **ShiftRows** (cyclic row shifting), **MixColumns** (column mixing), **AddRoundKey** (combines the state with a round key). The final round omits MixColumns.

---

## 🔢 04. RSA — Rivest–Shamir–Adleman

| Property | Detail |
|---|---|
| Type | Asymmetric cryptosystem |
| Basis | Modular arithmetic / difficulty of factoring large composites |
| Common sizes covered | 2048, 3072, 4096 bits |

**Key generation**
```text
Choose large primes p and q → Compute R = p × q → Compute φ(R) →
Choose public exponent e → Compute private exponent d →
Public Key = {e, R}    Private Key = {d, p, q}
```
`65537` is identified as a commonly used public exponent.

---

## 📐 05. ECC — Elliptic Curve Cryptography

A public-key cryptographic approach based on elliptic curves over finite fields. Its security is based on the **elliptic curve discrete logarithm problem (ECDLP)** rather than RSA's integer-factorization problem.

**Curves covered:** P-256, P-384, Curve25519, secp256k1

**Uses:** ECDH (key exchange), ECDSA (digital signatures), EdDSA (digital signatures), encryption schemes

**Key-size comparison**

| Approx. Symmetric Security | RSA | ECC |
|---|---|---|
| 128-bit | 3072-bit | 256-bit |
| 192-bit | 7680-bit | 384-bit |
| 256-bit | 15360-bit | 521-bit |

Shorter ECC keys are highlighted as useful for mobile, IoT, and bandwidth/compute-constrained environments.

---

## 🧮 06. Cryptographic Hashing & SHA

A cryptographic hash takes an input of any length and produces a fixed-length, deterministic digest. Hashing is a one-way operation used for verification and integrity rather than confidentiality.

**Required hash properties:** pre-image resistance, second pre-image resistance, collision resistance.

**SHA family**

| Variant | Digest | Status / Use |
|---|---|---|
| SHA-1 | 160 bits | Deprecated and broken for security purposes |
| SHA-224 | 224 bits | SHA-2 variant |
| SHA-256 | 256 bits | Secure, widely adopted |
| SHA-384 | 384 bits | SHA-2 variant |
| SHA-512 | 512 bits | Secure SHA-2 variant |
| SHA-3 | 224–512 bits | Keccak-based sponge construction |

SHA-1 is explicitly identified as deprecated following practical collision demonstrations; SHA-256/SHA-512 are described as secure SHA-2 variants.

---

## 💢 07. MD5 — Message Digest Algorithm 5

MD5 was designed by Ronald Rivest in 1991 and produces a 128-bit / 32-hex-character digest. **MD5 is now cryptographically broken and must not be used for security-relevant purposes.**

Its primary cryptographic failure is **collision resistance** — practical methods can construct different inputs with the same MD5 digest, undermining integrity mechanisms that depend on digest uniqueness.

```text
MD5 Weaknesses
  ├── Broken collision resistance
  └── Extremely fast computation
```
These affect different use cases, with collision attacks being particularly relevant to signatures and integrity verification.

---

## 🔒 08–10. Password Hashing

Password storage requires deliberately expensive functions rather than fast general-purpose hashes.

```text
Password Protection
 ┌──────┼──────┐
bcrypt Argon2 PBKDF2
 Cost   Memory  Iterations
```

### 08. bcrypt

Designed in 1999 around the Blowfish cipher's key-setup algorithm. Defining feature: a **tunable exponential cost factor**, deliberately making password hashing slow to resist brute-force guessing. Salting is built into its format.

```text
cost = 10 → 2^10 = 1,024 rounds
cost = 12 → 2^12 = 4,096 rounds
cost = 14 → 2^14 = 16,384 rounds
```
The work factor can be increased as hardware becomes faster.

### 09. Argon2

A dedicated **memory-hard** password-hashing function, winner of the Password Hashing Competition in 2015.

**Variants:** Argon2d, Argon2i, Argon2id
**Tunable parameters:** memory cost, time cost/iterations, degree of parallelism

Contrasted with bcrypt's compute-hard, relatively memory-light design — Argon2 is compute-hard *and* memory-hard. Requiring substantial memory per hashing attempt increases the cost of large-scale parallel cracking.

### 10. PBKDF2

A key derivation function standardized in **RFC 2898 (2000)**. Repeatedly applies a pseudorandom function — commonly HMAC with a hash such as SHA-256 — to a password and random salt.

```text
DK = PBKDF2(PRF, Password, Salt, IterationCount, KeyLength)
```
The iteration count controls the deliberate computational cost.

> **Important limitation:** PBKDF2 has no memory-hardness parameter, unlike Argon2 — its major limitation relative to newer password-hashing designs.

**Password-hashing comparison**

| Property | bcrypt | Argon2 | PBKDF2 |
|---|---|---|---|
| Primary role | Password hashing | Password hashing | KDF / password hashing |
| Designed | 1999 | 2015 | 2000 |
| Main cost control | Exponential work factor | Memory + time + parallelism | Iteration count |
| Memory-hard | Relatively memory-light | Yes | No |
| Salt support | Built into format | Supported | Uses salt |

---

## ✒️ 11. Digital Signatures

A digital signature authenticates a message/document and provides evidence that it has not been altered. Uses asymmetric cryptography: **private key signs, public key verifies.**

Provides: **authentication, integrity, non-repudiation.** A digital signature is not encryption and does not hide the content.

**Signing workflow**
```text
Message → Hash Function → Message Digest → Private-Key Signing →
Digital Signature → Message + Signature
```
Hashing is used before signing because signing large raw messages directly with an asymmetric algorithm would be inefficient.

**Algorithms covered:** RSA with PSS padding, ECDSA, EdDSA/Ed25519

---

## 🏛️ 12. PKI — Public Key Infrastructure

PKI is the system of policies, roles, procedures, hardware, and software used to create, manage, distribute, use, store, and revoke digital certificates and manage public-key cryptography at scale.

**The problem PKI solves:** a public key by itself does not prove who owns it. An attacker who substitutes their own public key can undermine the guarantees of asymmetric cryptography. PKI addresses this key-distribution and identity-binding problem through trusted attestations.

**Core participants**

| Participant | Role |
|---|---|
| Certificate Authority (CA) | Verifies identity and signs certificates |
| Registration Authority (RA) | Performs identity verification for the CA |
| End Entity / Subscriber | Identity whose public key is being certified |

The CA digitally signs certificates to vouch for the identity/public-key binding.

---

## 📜 13. Digital Certificates

A digital certificate binds a public key to an identity and is digitally signed by a trusted Certificate Authority. **X.509** is the dominant certificate standard in modern use.

**Important certificate fields**

| Field | Purpose |
|---|---|
| Subject | Entity identified by the certificate |
| Subject Public Key | Public key bound to that identity |
| Issuer | CA that issued the certificate |
| Serial Number | Unique certificate identifier |
| Validity Period | Not Before / Not After dates |
| Signature Algorithm | Algorithm used by the CA |
| CA Signature | CA's signature over certificate information |
| Extensions | Additional information such as SANs and key usage |

Subject Alternative Names (SANs) can allow one certificate to cover multiple domains.

---

## 🌐 14. TLS & HTTPS

TLS (Transport Layer Security) secures data in transit, providing **confidentiality, integrity, and authentication.** HTTPS is HTTP running over a TLS-secured connection.

**What TLS provides:**
- **Confidentiality** — application data protected with symmetric encryption, typically AES.
- **Integrity** — tampering detectable using authenticated encryption such as AES-GCM or a MAC.
- **Authentication** — the server, and optionally the client, proves identity through a certificate chain leading to a trusted CA.

**TLS 1.3** is described as a simplified and faster handshake compared with TLS 1.2, with legacy/insecure options removed and fewer round trips.

```text
Client → ClientHello → Server
Server → ServerHello + Certificate + Authentication/Key Exchange → Client
Client → Secure Symmetric Session → Encrypted Application Data
```

---

## 🔗 How the Topics Connect

The 13 PDFs form one practical cryptography stack:

```text
                    CRYPTOGRAPHY
        ┌────────────────┼────────────────┐
   Encryption         Hashing       Authentication
   ┌────┴────┐      ┌────┴────┐     Digital
   │         │      │         │     Signatures
 AES       RSA     SHA       MD5
             │
            ECC
             │
       Public-Key Crypto
             │
            PKI
             │
     Digital Certificates
             │
            TLS
             │
           HTTPS
```

Password protection is a related branch:
```text
Password
   ├── bcrypt  → tunable compute cost
   ├── Argon2  → memory + time + parallelism
   └── PBKDF2  → repeated PRF iterations
```

TLS is explicitly described as the convergence point of certificates, PKI, digital signatures, asymmetric key exchange, and symmetric bulk encryption.

---

## ⚖️ Encryption vs. Hashing vs. Password Hashing vs. Signatures

| Mechanism | Main Purpose | Examples |
|---|---|---|
| Encryption | Confidentiality | AES, RSA/ECC-based schemes |
| Cryptographic Hash | Integrity / verification | SHA-256, SHA-512 |
| Password Hash / KDF | Resist password guessing / derive keys | bcrypt, Argon2, PBKDF2 |
| Digital Signature | Authentication, integrity, non-repudiation | RSA-PSS, ECDSA, EdDSA |

A major distinction: **digital signatures do not provide confidentiality.**

---

## 📋 Algorithm Status Cheat Sheet

| Technology | Category | Key Point |
|---|---|---|
| AES | Symmetric encryption | Modern block cipher |
| RSA | Asymmetric cryptography | Public/private-key system |
| ECC | Asymmetric cryptography | Strong security with shorter keys |
| SHA-256 | Cryptographic hash | Secure, widely adopted |
| SHA-512 | Cryptographic hash | Secure SHA-2 variant |
| SHA-3 | Cryptographic hash | Keccak-based sponge construction |
| SHA-1 | Cryptographic hash | Deprecated / broken for security |
| MD5 | Cryptographic hash | Broken; not for security |
| bcrypt | Password hashing | Tunable compute cost |
| Argon2 | Password hashing | Memory-hard |
| PBKDF2 | KDF / password hashing | Iteration-based |
| RSA-PSS | Digital signature | RSA signature scheme |
| ECDSA | Digital signature | ECC-based signature |
| EdDSA / Ed25519 | Digital signature | Elliptic-curve signature |
| X.509 | Certificate standard | Dominant certificate format |
| TLS 1.2 | Secure transport | Secure TLS version covered |
| TLS 1.3 | Secure transport | Modern simplified handshake |
| SSL | Transport predecessor | Obsolete |

SHA-1 and MD5 are explicitly marked as unsuitable for security purposes. TLS 1.2 and TLS 1.3 are current versions in use, while TLS 1.0/1.1 are deprecated and SSL is obsolete.

---

## 📖 Recommended Study Order

```text
01. Cryptography Fundamentals → 02. AES → 03. RSA → 04. ECC → 05. SHA → 06. MD5 →
07. bcrypt → 08. Argon2 → 09. PBKDF2 → 10. Digital Signatures →
11. PKI → 12. Digital Certificates → 13. TLS & HTTPS
```

This order moves from fundamentals → encryption → public-key cryptography → hashing → password protection → signatures → trust infrastructure → secure network communication.

---

## ⚡ Quick Revision Cheat Sheet

| Topic | Pattern |
|---|---|
| Cryptography | Mathematical protection of information |
| AES | Symmetric + block cipher + 128-bit block + 128/192/256-bit keys |
| RSA | Asymmetric + modular arithmetic + factorization-based security |
| ECC | Asymmetric + ECDLP + shorter keys |
| SHA | One-way hash + fixed digest + integrity |
| MD5 | 128-bit hash + collision broken + not for security |
| bcrypt | Password hashing + salt + exponential cost |
| Argon2 | Password hashing + memory-hard + configurable resources |
| PBKDF2 | KDF + HMAC + salt + repeated iterations |
| Digital Signature | Private key signs + public key verifies |
| PKI | Trust framework + CA + identity/public-key binding |
| Digital Certificate | Identity + public key + CA signature + X.509 |
| TLS | Confidentiality + integrity + authentication |
| HTTPS | HTTP + TLS |

---

## 📁 Folder Structure

```text
12_Cryptography
│
├── 01. Cryptography.pdf
├── 02. AES (Advanced Encryption Standard).pdf
├── 03. RSA (Rivest–Shamir–Adleman).pdf
├── 04. ECC (Elliptic Curve Cryptography).pdf
├── 05. SHA (Secure Hash Algorithm).pdf
├── 06. MD5 (Message Digest Algorithm 5).pdf
├── 07. bcrypt.pdf
├── 08. Argon2.pdf
├── 09. PBKDF2 (Password-Based Key Derivation Function 2).pdf
├── 10. Digital Signatures.pdf
├── 11. PKI (Public Key Infrastructure).pdf
├── 12. Digital Certificates.pdf
├── 13. TLS & HTTPS.pdf
└── README.md
```

---

## 🔗 Related Cybersecurity Topics

🔐 Network Security · 🌐 HTTPS / Web Security · 🛡️ Authentication · 🔑 Password Security · 🏢 Active Directory Security · 📡 Secure Network Protocols · 🔎 Digital Forensics · 🧪 Vulnerability Assessment · 🕵️ SOC / Threat Detection · 📜 Digital Identity · 🔏 Certificate Management · 🧰 Secure Application Development

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

This repository is intended for cybersecurity education, defensive security research, secure software development, cryptographic study, and authorized lab environments. Cryptographic attacks, password-security testing, and protocol analysis should only be performed on systems, applications, and data for which you have explicit authorization.
