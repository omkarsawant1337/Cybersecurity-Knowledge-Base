# 🏛️ Compliance & Governance

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Level](https://img.shields.io/badge/level-intermediate-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

A structured cybersecurity knowledge base covering security, compliance, risk management, governance, privacy, assurance, and security control frameworks.

This repository brings together study material for **HIPAA, GDPR, PCI DSS, ISO/IEC 27001, SOC 2, SOX, NIST CSF 2.0, and CIS Controls.**

> ⚠️ **Important:** Compliance is not the same thing as security. Compliance establishes a required or verifiable baseline, while effective security continuously addresses the organization's actual threats and risk profile.

---

## 📖 Table of Contents

- [Repository Contents](#-repository-contents)
- [Learning Objectives](#-learning-objectives)
- [01. Compliance vs. Security](#️-01-compliance-vs-security)
- [Why Compliance Exists](#-why-compliance-exists)
- [Framework Landscape](#-framework-landscape)
- [02. HIPAA](#-02-hipaa)
- [03. GDPR](#-03-gdpr)
- [04. PCI DSS](#-04-pci-dss)
- [05. ISO/IEC 27001](#-05-isoiec-27001)
- [06. SOC 2](#-06-soc-2)
- [07. SOX — Sarbanes-Oxley Act](#-07-sox--sarbanes-oxley-act)
- [08. NIST Cybersecurity Framework (CSF) 2.0](#-08-nist-cybersecurity-framework-csf-20)
- [09. CIS Controls](#-09-cis-controls)
- [How the Frameworks Fit Together](#-how-the-frameworks-fit-together)
- [Framework Comparison Cheat Sheet](#-framework-comparison-cheat-sheet)
- [Common Compliance Failure Patterns](#-common-compliance-failure-patterns)
- [Common Framework-Specific Gaps](#-common-framework-specific-gaps)
- [Governance Evidence Model](#-governance-evidence-model)
- [Practical Control-Mapping Model](#-practical-control-mapping-model)
- [Recommended Study Order](#-recommended-study-order)
- [Interview / Exam Quick Revision](#-interview--exam-quick-revision)
- [One-Page Mental Model](#-one-page-mental-model)
- [Key Takeaways](#-key-takeaways)
- [Folder Structure](#-folder-structure)
- [Framework Selection — Simplified](#-framework-selection--simplified)
- [Related Cybersecurity Topics](#-related-cybersecurity-topics)
- [Author](#-author)
- [License](#-license)
- [Legal & Ethical Note](#️-legal--ethical-note)

---

## 📚 Repository Contents

| # | Document | Primary Focus |
|---|---|---|
| 01 | Compliance vs. Security | Difference between compliance and real-world security |
| 02 | HIPAA | Healthcare privacy/security and PHI/ePHI |
| 03 | GDPR | EU/EEA personal-data protection and privacy |
| 04 | PCI DSS | Payment-card security and Cardholder Data Environment |
| 05 | ISO 27001 | Risk-based ISMS, PDCA, Annex A, certification |
| 06 | SOC 2 | Service-organization assurance and Trust Services Criteria |
| 07 | SOX (Sarbanes-Oxley Act) | Financial reporting, ITGCs, and segregation of duties |
| 08 | NIST Cybersecurity Framework (CSF) | Organization-wide cybersecurity risk management |
| 09 | CIS Controls | Prioritized, attack-informed security safeguards |

---

## 🎯 Learning Objectives

After completing this section, you should be able to:

- Explain the difference between compliance, governance, risk management, and security.
- Understand why compliance should be treated as a baseline rather than the final security objective.
- Identify legal/regulatory, contractual, voluntary, and attestation-based frameworks.
- Understand the purpose and scope of HIPAA, GDPR, PCI DSS, ISO 27001, SOC 2, SOX, NIST CSF, and CIS Controls.
- Compare ISO 27001 certification with SOC 2 attestation.
- Understand risk assessment, control selection, evidence, audits, and continuous improvement.
- Distinguish NIST CSF's high-level structure from the more prescriptive CIS Controls.
- Recognize common compliance gaps and "compliance theater" patterns.
- Build a practical framework for mapping security controls to governance and compliance requirements.

---

## ⚖️ 01. Compliance vs. Security

| Compliance | Security |
|---|---|
| Meeting defined external requirements | Actual protection against realistic threats |
| Driven by law, regulation, contract, or standard | Driven by threat model, risk assessment, and risk tolerance |
| Measured by audits, certifications, and evidence | Measured by control effectiveness and resistance to real attacks |
| Often point-in-time | Continuous and adaptive |
| Audience includes regulators, auditors, customers, partners | Audience includes defenders and organizational risk owners |
| Failure can mean fines, contractual problems, or legal liability | Failure can mean breaches, data loss, disruption, and reputational damage |

**Core principle:**
```text
Compliance = Verifiable Baseline
Security   = Continuous Risk Reduction
```

An organization can be compliant and still be insecure. Likewise, strong technical security does not automatically satisfy every legal or contractual requirement. Compliance is a **floor, not a ceiling**: frameworks can lag emerging threats, audits are point-in-time/sample-based, and narrow assessment scopes can leave connected systems unaddressed.

```text
              BROADER SECURITY PROGRAM
        ┌──────────────┴──────────────┐
 Compliance Obligations        Security Beyond Compliance
        └──────────────┬──────────────┘
                       ↓
              Continuous Risk Reduction
```

---

## 🌐 Why Compliance Exists

- **Standardization** — customers, partners, and regulators can use common requirements instead of independently auditing every organization.
- **Accountability** — organizations maintain documented evidence showing that required controls were considered and implemented.
- **Legal & Contractual Necessity** — some frameworks carry direct legal, financial, regulatory, or contractual consequences.
- **Risk Reduction at Scale** — broadly applied frameworks can reduce aggregate risk across an industry or ecosystem.

---

## 🗂️ Framework Landscape

| Framework | Nature | Primary Domain |
|---|---|---|
| HIPAA | U.S. federal law | Healthcare information / PHI |
| GDPR | EU regulation | Personal data / privacy |
| PCI DSS | Industry/contractual standard | Payment-card data |
| ISO/IEC 27001 | Voluntary international standard | Information-security management |
| SOC 2 | Voluntary attestation framework | Service-organization controls |
| SOX | U.S. federal law | Financial reporting / corporate governance |
| NIST CSF 2.0 | Voluntary framework | Cybersecurity risk management |
| CIS Controls | Prescriptive control framework | Practical cyber defense |

---

## 🏥 02. HIPAA

**HIPAA** (Health Insurance Portability and Accountability Act) is a U.S. federal law governing privacy and security of health information. Its major security/privacy components are the **Privacy Rule** and **Security Rule**, strengthened by HITECH and the Omnibus Rule.

**Who must comply?**
- **Covered Entities** — healthcare providers, health plans, healthcare clearinghouses.
- **Business Associates** — cloud providers, billing services, IT vendors, SaaS platforms, and other third parties handling PHI on behalf of covered entities.

**What HIPAA protects:** **PHI** (Protected Health Information) includes individually identifiable health information. **ePHI** is PHI in electronic form and is the specific focus of the Security Rule.

**Privacy Rule concepts:** patient access to health information, rights to review and request corrections, permitted uses and disclosures, patient authorization, the minimum necessary standard.

**Security Rule safeguards**

| Category | Examples |
|---|---|
| Administrative | Risk analysis, workforce training, access management, contingency planning |
| Physical | Facility access, workstation security, device/media controls |
| Technical | Unique IDs, automatic logoff, audit controls, integrity, transmission security |

> **Addressable does not mean optional.** An addressable specification must be assessed for reasonableness/appropriateness and handled/documented accordingly.

**Breach & vendor governance:** HIPAA includes breach-notification obligations and requires appropriate Business Associate Agreements (BAAs) with vendors handling PHI. A BAA creates contractual obligations but does not by itself guarantee adequate technical security.

---

## 🇪🇺 03. GDPR

**GDPR** (General Data Protection Regulation) is an EU regulation governing the processing and protection of personal data. A defining characteristic is its **extraterritorial reach** — it can apply to organizations outside the EU/EEA when they offer goods/services to people in the EU or monitor their behavior.

**Key roles**

| Role | Definition |
|---|---|
| Data Subject | Individual whose data is processed |
| Data Controller | Determines purposes and means of processing |
| Data Processor | Processes data for the controller |
| DPO | Data Protection Officer responsible for GDPR oversight where required |

**Seven core principles:** lawfulness/fairness/transparency, purpose limitation, data minimization, accuracy, storage limitation, integrity and confidentiality, accountability.

**Six lawful bases:** consent, contract, legal obligation, vital interests, public task, legitimate interests.

**Data subject rights:** access, rectification, erasure, restriction of processing, data portability, objection.

**Operational governance:** Data Protection Impact Assessments (DPIAs), legal-basis documentation, processor/vendor contracts, breach notification, international data transfers.

GDPR requires notifying the relevant supervisory authority within **72 hours** where feasible after becoming aware of a personal-data breach, with direct notification to affected individuals where the breach presents a high risk.

---

## 💳 04. PCI DSS

**PCI DSS** (Payment Card Industry Data Security Standard) is an industry/contractual standard maintained by the PCI Security Standards Council, applying to entities that store, process, or transmit payment-card data.

**Data covered:**
- **Cardholder Data (CHD)** — PAN, cardholder name, expiration date, service code.
- **Sensitive Authentication Data (SAD)** — full magnetic-stripe data, CAV2/CVC2/CVV2/CID, PINs/PIN blocks.

**Twelve core requirements:**
1. Install and maintain network security controls.
2. Apply secure configurations.
3. Protect stored account data.
4. Protect cardholder data during transmission over public networks.
5. Protect systems against malicious software.
6. Develop and maintain secure systems/software.
7. Restrict access by business need-to-know.
8. Identify users and authenticate access.
9. Restrict physical access.
10. Log and monitor access.
11. Test security regularly.
12. Support security with organizational policies/programs.

**Cardholder Data Environment (CDE)**
```text
People + Processes + Technology
            ↓
Systems storing/processing/transmitting card data
            +
Connected systems that can impact CDE security
```
Accurate scope definition and, where possible, network segmentation are important parts of a PCI DSS program.

**Compensating controls** — alternative controls may be used when a requirement cannot be met exactly because of a legitimate constraint. They must be documented, justified, meet the intent and rigor of the original requirement, and remain subject to assessor review.

---

## 🌍 05. ISO/IEC 27001

**ISO/IEC 27001** is a voluntary international standard for establishing, implementing, maintaining, and continually improving an **Information Security Management System (ISMS)**. Unlike fixed technical checklists, it focuses on a risk-based management process:

```text
Risk Assessment → Risk Treatment → Select Appropriate Controls → Operate & Monitor → Improve
```

**ISMS** brings together policies, procedures, risk assessments, risk treatment, controls, monitoring, and improvement.

**PDCA Cycle**
```text
PLAN → DO → CHECK → ACT
  ↑              │
  └──────────────┘
```
- **Plan:** define scope, assess risk, select controls, set objectives.
- **Do:** implement controls, resources, and training.
- **Check:** monitor, audit, review, and measure effectiveness.
- **Act:** correct nonconformities and continually improve.

**Risk treatment**

| Approach | Meaning |
|---|---|
| Mitigate | Implement controls to reduce risk |
| Accept | Formally accept the risk |
| Avoid | Eliminate the risk-generating activity |
| Transfer | Shift some risk through mechanisms such as insurance/outsourcing |

**Statement of Applicability (SoA)** — documents which Annex A controls are applicable and the rationale for inclusion/exclusion.

**Annex A themes:** organizational controls, people controls, physical controls, technological controls.

---

## ✅ 06. SOC 2

**SOC 2** (System and Organization Controls 2) is a voluntary AICPA attestation framework designed primarily for service organizations, especially SaaS and cloud providers. SOC 2 produces an **independent auditor's attestation report**, not a certification.

**Trust Services Criteria**

| Criterion | Status | Focus |
|---|---|---|
| Security | Mandatory | Protection against unauthorized access |
| Availability | Optional | System availability |
| Processing Integrity | Optional | Complete, valid, accurate, timely, authorized processing |
| Confidentiality | Optional | Protection of confidential information |
| Privacy | Optional | Handling personal information according to commitments |

**Type I vs. Type II**

| Type I | Type II |
|---|---|
| Point-in-time assessment | Observation-period assessment |
| Control design | Design + operating effectiveness |
| Snapshot | Sustained evidence |
| Lower relative rigor | Higher relative rigor |

A Type II report provides stronger evidence that controls actually operated effectively over time.

---

## 📊 07. SOX — Sarbanes-Oxley Act

**SOX** is a U.S. federal law enacted in 2002 to improve the accuracy and reliability of corporate financial reporting. Its IT-security implications arise because financial reporting depends on secure, reliable, and controlled information systems.

**Important sections**

| Section | Focus |
|---|---|
| 302 | Executive responsibility for financial reports and disclosure controls |
| 404 | Management must establish and assess internal controls over financial reporting — creates much of the practical IT compliance burden |
| 409 | Rapid/current disclosure of material changes |
| 802 | Criminal consequences for knowingly altering, destroying, or falsifying records to obstruct investigations |

**IT General Controls (ITGCs)**

| Domain | Focus |
|---|---|
| Access Controls | Authorized access and segregation of duties |
| Change Management | Authorized, tested, documented production changes |
| Operations Management | Reliable operations, backups, incidents, scheduling |
| Program Development / SDLC | Controlled development and implementation |

**Segregation of duties:** no single individual should have end-to-end control over a financial process in a way that allows improper activity to be performed and concealed. Example: `Create Vendor ≠ Approve Vendor Payment`.

---

## 🧭 08. NIST Cybersecurity Framework (CSF) 2.0

NIST CSF is a **voluntary, non-certifiable** framework developed by NIST, providing a common language and structured approach for managing cybersecurity risk across organizations of different sizes and sectors.

**Six core functions:**
```text
Govern → Identify → Protect → Detect → Respond → Recover ↺
```
- **Govern** — establish and monitor cybersecurity risk strategy, expectations, and policy.
- **Identify** — understand assets, systems, data, risks, and business context.
- **Protect** — implement safeguards.
- **Detect** — identify cybersecurity events.
- **Respond** — contain and manage incidents.
- **Recover** — restore capabilities and resilience.

These functions are not a strict sequence — organizations operate across them continuously.

**Structure:** `Function → Category → Subcategory`

**Implementation Tiers:** Tier 1 — Partial, Tier 2 — Risk Informed, Tier 3 — Repeatable, Tier 4 — Adaptive. Tiers describe organizational cybersecurity risk-management sophistication rather than a simple control score.

**Profiles:** `Current Profile → Gap Analysis → Target Profile → Prioritized Improvement`

NIST CSF is commonly useful as an organizing layer above more detailed control sets.

---

## 🛡️ 09. CIS Controls

The **CIS Controls** are a prescriptive, prioritized set of technical and organizational safeguards maintained by the Center for Internet Security, strongly informed by real-world attack patterns.

**The 18 Controls:**
1. Inventory and Control of Enterprise Assets
2. Inventory and Control of Software Assets
3. Data Protection
4. Secure Configuration of Enterprise Assets and Software
5. Account Management
6. Access Control Management
7. Continuous Vulnerability Management
8. Audit Log Management
9. Email and Web Browser Protections
10. Malware Defenses
11. Data Recovery
12. Network Infrastructure Management
13. Network Monitoring and Defense
14. Security Awareness and Skills Training
15. Service Provider Management
16. Application Software Security
17. Incident Response Management
18. Penetration Testing

Asset/software inventory and foundational hygiene are placed deliberately early: organizations cannot reliably secure, patch, or monitor assets they do not know exist.

**Implementation Groups**

| Group | Target Profile |
|---|---|
| IG1 | Small/resource-limited organizations; essential cyber hygiene |
| IG2 | Organizations with dedicated IT/security teams |
| IG3 | Larger, sophisticated/high-risk organizations |

```text
IG1 (Essential Hygiene) → IG2 (Additional Safeguards) → IG3 (Advanced Safeguards)
```
IG1 should be completed first, rather than jumping to advanced safeguards.

**CIS Controls vs. CIS Benchmarks:** Controls define *what* categories of safeguards an organization should implement; Benchmarks provide *product/platform-specific* technical hardening guidance.

---

## 🧩 How the Frameworks Fit Together

```text
                 GOVERNANCE
                     │
                NIST CSF 2.0
          Risk & Organizational Structure
                     │
          ┌──────────┴──────────┐
      ISO 27001            CIS Controls
      Risk-Based ISMS      Technical Safeguards
          └──────────┬──────────┘
                     ▼
              SECURITY PROGRAM
                     │
       ┌─────────────┼─────────────┐
    HIPAA          GDPR          PCI DSS
   Healthcare     Privacy       Card Data
       └─────────────┼─────────────┘
                     ▼
             Assurance & Evidence
                     │
                SOC 2 / SOX
```

This is a study model, not a requirement that every organization implement every framework.

---

## 📋 Framework Comparison Cheat Sheet

| Framework | Primary Concern | Output / Nature | Typical Scope |
|---|---|---|---|
| HIPAA | Healthcare privacy/security | Regulation | PHI/ePHI |
| GDPR | Privacy/personal data | Regulation | EU/EEA personal data |
| PCI DSS | Payment-card security | Industry standard | CDE |
| ISO 27001 | Information-security management | Certification | Defined ISMS scope |
| SOC 2 | Service-provider controls | Attestation report | Defined service/system scope |
| SOX | Financial reporting integrity | Regulation | Financial reporting environment |
| NIST CSF | Cyber risk management | Voluntary framework | Organization-wide |
| CIS Controls | Practical cyber defense | Prescriptive safeguards | Organization-wide |

---

## ⚠️ Common Compliance Failure Patterns

**Compliance Theater** — controls exist mainly to satisfy an audit rather than reduce real risk: documentation doesn't match operational reality, controls are maintained only during audit periods, weak implementations technically satisfy checklist items, high-risk connected systems are excluded through narrow scoping, certification is treated as the end of the process.

**Point-in-Time Thinking** — a successful audit does not prove continuous effectiveness:
```text
Audit → Evidence → Continuous Operation → Monitoring → Improvement
```

**Scope Problems** — poor scope definition can under-cover real risk or create unnecessary compliance burden.

**Documentation Without Reality** — a mature program should connect:
```text
Policy → Procedure → Implementation → Evidence → Testing → Monitoring
```

---

## 🔍 Common Framework-Specific Gaps

**HIPAA:** inadequate risk analysis · weak vendor/BAA governance · poor breach-response readiness · treating addressable safeguards as optional · documentation that doesn't match technical controls

**GDPR:** poor legal-basis documentation · weak consent mechanisms · missing/incomplete DPIAs · poor international-transfer documentation · untested breach-notification processes · incomplete processor contracts

**PCI DSS:** incorrect CDE scope · weak segmentation · poor vulnerability management · inadequate logging/monitoring · weakly justified compensating controls

**ISO 27001:** audit-focused Statement of Applicability · documentation disconnected from operations · weak internal audits · treating certification as a one-time achievement · narrow ISMS scope

**SOC 2:** relying on Type I when Type II is expected · selecting criteria too narrowly · controls weakening outside the audit period · outdated system descriptions · underestimating continuous evidence collection

**SOX:** weak segregation of duties · poor access reviews · uncontrolled production changes · weak IT operations controls · inadequate audit-log integrity

**NIST CSF:** treating CSF as a documentation exercise · underinvesting in Govern · confusing CSF alignment with certification · limiting it to the technical security team

**CIS Controls:** attempting IG2/IG3 before completing IG1 · stale asset/software inventories · configuration drift after hardening · selecting IGs based on aspiration rather than actual risk/resources

---

## 🔗 Governance Evidence Model

```text
Risk → Requirement → Control → Owner → Implementation →
Evidence → Testing → Finding → Remediation → Continuous Monitoring
```

This turns compliance evidence into an operational security capability rather than a collection of disconnected documents.

---

## 🗺️ Practical Control-Mapping Model

| Layer | Example |
|---|---|
| Business Risk | Customer-data breach |
| Governance Objective | Protect sensitive information |
| Framework Requirement | Applicable privacy/security requirement |
| Security Control | Access control + encryption |
| Implementation | IAM, encryption, logging |
| Evidence | Access review, configuration, logs |
| Testing | Internal/external assessment |
| Remediation | Fix identified gap |
| Monitoring | Continuous control monitoring |

The same underlying security control can support multiple compliance obligations.

---

## 📖 Recommended Study Order

```text
01. Compliance vs. Security → 02. Framework Types → 03. HIPAA → 04. GDPR →
05. PCI DSS → 06. ISO 27001 → 07. SOC 2 → 08. SOX / ITGC →
09. NIST CSF 2.0 → 10. CIS Controls → 11. Cross-Framework Mapping →
12. Governance & Continuous Improvement
```

---

## 🎓 Interview / Exam Quick Revision

**Q: Is compliance the same as security?**
No. Compliance is a verifiable baseline; security is continuous protection against realistic threats.

**Q: What does HIPAA protect?**
Protected Health Information (PHI), with the Security Rule specifically addressing ePHI.

**Q: What is GDPR mainly concerned with?**
Personal-data protection and privacy.

**Q: What are GDPR's seven principles?**
Lawfulness/fairness/transparency, purpose limitation, data minimization, accuracy, storage limitation, integrity/confidentiality, and accountability.

**Q: What is PCI DSS?**
An industry/contractual standard for protecting payment-card data.

**Q: What is the CDE?**
The people, processes, technology, and connected systems involved in or capable of affecting cardholder-data security.

**Q: What is ISO 27001?**
A risk-based standard for an Information Security Management System.

**Q: What is PDCA?**
Plan → Do → Check → Act.

**Q: What is the Statement of Applicability?**
A document explaining applicable Annex A controls and the organization's rationale.

**Q: What is SOC 2?**
An AICPA attestation framework for service organizations.

**Q: Type I vs. Type II?**
Type I is point-in-time control design; Type II evaluates design and operating effectiveness over an observation period.

**Q: What is SOX?**
A U.S. federal law focused on financial-reporting integrity and corporate governance.

**Q: What are ITGCs?**
IT General Controls supporting reliable financial-reporting systems.

**Q: What are NIST CSF 2.0's six functions?**
Govern, Identify, Protect, Detect, Respond, Recover.

**Q: Does NIST CSF provide certification?**
No.

**Q: What are CIS Controls?**
Prioritized, prescriptive cybersecurity safeguards informed by real-world attacks.

**Q: Which CIS Implementation Group should normally come first?**
IG1 — foundational cyber hygiene.

---

## 🧠 One-Page Mental Model

```text
                 COMPLIANCE & GOVERNANCE
              ┌────────────┴────────────┐
          WHY / WHAT               HOW / EVIDENCE
        Regulations                 Policies
        Standards                   Controls
        Contracts                   Procedures
        Risk                        Evidence
        Governance                  Audits
              └────────────┬────────────┘
                           ↓
                    SECURITY PROGRAM
       ┌───────────────────┼───────────────────┐
   Protect Data        Manage Risk        Prove Controls
 HIPAA / GDPR         NIST / ISO           SOC 2 / SOX
       └───────────────────┼───────────────────┘
                           ↓
                     CIS Controls
                  Practical Safeguards
                           ↓
                  Continuous Improvement
```

---

## 🎯 Key Takeaways

1. Compliance and security are related but distinct.
2. Compliance establishes an externally verifiable baseline; security continuously manages actual threats and risk.
3. Passing an audit does not automatically mean an organization is secure.
4. Strong security does not automatically satisfy every legal or contractual obligation.
5. HIPAA focuses on healthcare information and PHI/ePHI.
6. GDPR focuses on personal data and privacy rights, with significant extraterritorial reach.
7. PCI DSS focuses on payment-card environments and cardholder data.
8. ISO 27001 focuses on a systematic, risk-based ISMS and continuous improvement.
9. SOC 2 provides independent assurance over service-organization controls.
10. SOX connects financial-reporting integrity with IT General Controls.
11. NIST CSF 2.0 provides organization-wide cybersecurity risk-management structure.
12. CIS Controls provide practical, prioritized safeguards based on real-world attack patterns.
13. NIST CSF and CIS Controls complement each other: CSF provides high-level structure while CIS provides actionable implementation detail.
14. CIS IG1 should be the foundation before moving toward more advanced safeguard groups.
15. Scope management is a major compliance concern.
16. Documentation must reflect actual operational practice.
17. Evidence should be collected continuously rather than only before audits.
18. Compliance should be embedded into a broader, continuously improving security program.

---

## 📁 Folder Structure

```text
15_Compliance_&_Governance
│
├── 01. Compliance vs. Security.pdf
├── 02. HIPAA.pdf
├── 03. GDPR.pdf
├── 04. PCI DSS.pdf
├── 05. ISO 27001.pdf
├── 06. SOC 2.pdf
├── 07. SOX (Sarbanes-Oxley Act).pdf
├── 08. NIST Cybersecurity Framework (CSF).pdf
├── 09. CIS Controls.pdf
└── README.md
```

---

## 🧭 Framework Selection — Simplified

| Primary Concern | Starting Point |
|---|---|
| Healthcare PHI/ePHI | HIPAA |
| EU/EEA personal data | GDPR |
| Payment-card data | PCI DSS |
| Enterprise information-security management | ISO 27001 |
| SaaS/service-provider assurance | SOC 2 |
| Financial reporting controls | SOX |
| Organization-wide cyber risk management | NIST CSF 2.0 |
| Practical prioritized safeguards | CIS Controls |

These are study-oriented starting points, not legal advice or a substitute for determining the exact obligations applicable to a specific organization.

---

## 🔗 Related Cybersecurity Topics

🔐 Information Security Management · 🏛️ Security Governance · ⚖️ Regulatory Compliance · 📋 Risk Management · 🔎 Audit & Assurance · 🛡️ Security Controls · 🔐 Privacy Engineering · 🧑‍💼 IT Governance · 🧾 IT General Controls · 📊 Risk Assessment · 🔄 Continuous Improvement · 🧪 Control Testing · 📑 Security Policies · 🔍 Vulnerability Management · 🚨 Incident Response · 📦 Third-Party Risk Management · ☁️ Cloud Compliance · 🔗 Control Mapping

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

This repository is intended for cybersecurity education, defensive security, governance, compliance preparation, audit readiness, and authorized security work. Compliance obligations vary by jurisdiction, industry, organization type, data handled, contracts, and system scope. **Use this repository as a learning/reference resource rather than legal advice.**
