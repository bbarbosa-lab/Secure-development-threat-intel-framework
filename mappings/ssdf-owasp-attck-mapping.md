# SSDF × OWASP × MITRE ATT&CK Mapping

This is the core artifact of this repository. It shows how practices from **NIST SSDF v1.1** directly help mitigate specific risks from **OWASP Top 10 2025** and **OWASP Top 10 API Security Risks 2023**, while addressing real attacker techniques from **MITRE ATT&CK**.

## How to Use This Mapping

1. When designing a new feature/endpoint → look at relevant ATT&CK techniques + OWASP risks.
2. When planning your secure development process → use SSDF practices as the structure.
3. When prioritizing technical debt → map findings to this table to justify effort with business + threat context.

---

## Core Mapping Table (Simplified View)

| NIST SSDF Practice | OWASP Web/API Risk(s) Mitigated | Relevant MITRE ATT&CK Techniques | Threat Intel Input | Concrete Implementation Example |
|--------------------|----------------------------------|----------------------------------|--------------------|---------------------------------|
| **PO.1** — Define Security Requirements | Broken Access Control, Broken Authentication, BOLA (API #1) | T1078 Valid Accounts, T1190 Exploit Public-Facing App | ATT&CK Navigator for your tech stack | Threat model every new endpoint using ATT&CK + OWASP checklist |
| **PO.2** — Identify and Assess Risks | All OWASP risks | All relevant TTPs | MISP + Talos recent campaigns | Risk assessment that includes "Which ATT&CK techniques are active against this type of app?" |
| **PS.1** — Protect Sensitive Data | Cryptographic Failures, Sensitive Data Exposure | T1552 Unsecured Credentials, T1005 Data from Local System | NVD (new crypto CVEs) | Enforce encryption at rest/transit + key management (MASVS Crypto for mobile) |
| **PS.2** — Manage Security of 3rd Party Components | Software Supply Chain Failures (OWASP #3 2025), Unsafe Consumption of APIs (API #10) | T1195 Supply Chain Compromise | NVD + Snyk/Grype + MISP IOCs for malicious packages | Automated dependency scanning + CVE gate in CI/CD (see examples/nvd-dependency-check) |
| **PS.3** — Secure Build & Deployment | Security Misconfiguration (API #8), Improper Inventory Management (API #9) | T1078, T1190 | CIS Benchmarks + Talos rules | Hardened Docker/K8s images + API Gateway policies + inventory of all public endpoints |
| **PS.4** — Protect Code & Configuration | Injection flaws, Security Misconfiguration | T1059 Command and Scripting Interpreter, T1190 | — | Input validation, parameterized queries, principle of least privilege in code |
| **PW.1** — Secure Coding Practices | Injection, Broken Access Control, BOLA | T1059, T1190, T1552 | — | Code review checklist mapped to OWASP + static analysis (SAST) |
| **PW.2** — Review & Test Code | All OWASP risks | — | — | Threat-informed test cases (including abuse cases from ATT&CK) + DAST + IAST |
| **PW.3** — Manage Technical Debt | All (especially recurring ones) | — | NVD trending CVEs | Backlog prioritized by CVSS + exploitability + business impact |
| **RV.1** — Respond to Vulnerabilities | All | — | NVD + MISP + Talos | Automated patching pipeline + SLA based on severity + threat intel |
| **RV.2** — Respond to Incidents | All | All (post-incident learning) | FIRST + SANS ISC + internal telemetry | Structured logging (STIX-ready) + playbooks + feedback loop to threat modeling |

---

## Detailed Example: BOLA (Broken Object Level Authorization) — OWASP API #1 2023

**Why it matters**: Most common and easy to exploit API vulnerability. Attacker changes object ID in request to access other users' data.

**SSDF Practices that mitigate it**:
- PO.1 (Security Requirements) → Explicit authorization policy per object
- PW.1 (Secure Coding) → Always verify `token.user_id == requested_object.owner_id`
- PW.2 (Review & Test) → Automated tests with different user contexts + property-based testing

**ATT&CK Techniques addressed**:
- T1078 Valid Accounts (abusing legitimate sessions)
- T1190 Exploit Public-Facing Application

**Threat Intel Input**:
- Watch MISP/Talos for campaigns targeting IDOR/BOLA in similar apps
- Study real breach reports (haveibeenpwned, Verizon DBIR, etc.)

**Implementation**:
See `examples/fastapi-threat-shield/` for middleware pattern + authorization decorator example.

---

## How to Extend This Mapping

This table is a starting point. In a real organization you should:

1. Customize ATT&CK techniques to your specific technology stack and industry.
2. Add columns for compliance frameworks you care about (LGPD, GDPR, PCI-DSS, ISO 27001, etc.).
3. Link each cell to internal tickets, policies, or code examples.

This mapping turns abstract frameworks into **actionable decisions** during architecture, coding, and pipeline design.
