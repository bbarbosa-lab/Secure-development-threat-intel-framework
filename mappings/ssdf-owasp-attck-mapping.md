# SSDF × OWASP × MITRE ATT&CK Mapping

This is one of the most valuable artifacts in this repository. It provides a practical bridge between three major frameworks:

- **NIST SSDF v1.1** — The secure development lifecycle structure
- **OWASP Top 10 2025 + OWASP Top 10 API Security Risks 2023** — The most critical risks in web and API applications
- **MITRE ATT&CK** — Real attacker techniques observed in the wild

The goal of this mapping is to move beyond generic checklists and enable **threat-informed decision making** during architecture, development, and security governance.

---

## Why This Mapping Matters

Most organizations treat OWASP, SSDF, and ATT&CK as separate exercises. This document shows how they work together:

- **SSDF** provides the *when* (which phase of the lifecycle)
- **OWASP** provides the *what* (which risks to prioritize)
- **ATT&CK** provides the *how* (how real attackers exploit those risks)
- **Threat Intelligence** provides the *now* (which techniques are actively being used against organizations like yours)

When combined, these frameworks help teams make better decisions about where to invest limited security resources.

---

## How to Use This Mapping

| When you are...                    | Use this mapping to...                                                                 | Expected Outcome |
|------------------------------------|----------------------------------------------------------------------------------------|------------------|
| Designing a new feature or endpoint | Identify relevant OWASP risks + ATT&CK techniques                                      | Threat model with concrete attacker scenarios |
| Planning or improving your secure development process | Map current practices to SSDF and identify gaps                                        | Clear roadmap aligned with industry standards |
| Prioritizing technical debt or security findings | Justify effort using business impact + real threat context                             | Better prioritization and stakeholder communication |
| Building or reviewing security controls | Ensure controls address both OWASP risks and real attacker techniques                  | More effective and threat-informed controls |
| Conducting threat modeling         | Move from generic "what could go wrong" to specific attacker techniques                | Higher quality threat models |

---

## Core Mapping Table

| NIST SSDF Practice | Primary OWASP Risks Mitigated | Relevant MITRE ATT&CK Techniques | Threat Intelligence Input | Concrete Implementation Example |
|--------------------|-------------------------------|----------------------------------|---------------------------|---------------------------------|
| **PO.1** Define Security Requirements | Broken Access Control, BOLA (API #1), Broken Authentication | T1078 Valid Accounts, T1190 Exploit Public-Facing Application | ATT&CK Navigator filtered by your technology stack | Threat model every new endpoint using ATT&CK + OWASP checklist |
| **PO.2** Identify and Assess Risks | All OWASP risks | All relevant TTPs | MISP + Talos + sector-specific campaigns | Risk assessment that includes "Which ATT&CK techniques are currently active against this type of application?" |
| **PS.1** Protect Sensitive Data | Cryptographic Failures, Sensitive Data Exposure | T1552 Unsecured Credentials, T1005 Data from Local System | NVD (new crypto-related CVEs) | Enforce encryption at rest and in transit + secure key management (see MASVS Crypto for mobile) |
| **PS.2** Manage Security of 3rd Party Components | Software Supply Chain Failures (Web #3 2025), Unsafe Consumption of APIs (API #10) | T1195 Supply Chain Compromise | NVD + EPSS + MISP (malicious packages) | Automated dependency scanning + CVE gate in CI/CD (see `examples/nvd-dependency-check`) |
| **PS.3** Secure Build & Deployment | Security Misconfiguration (API #8), Improper Inventory Management (API #9) | T1078, T1190 | CIS Benchmarks + Talos rules + hardening guides | Hardened container images + API Gateway policies + complete inventory of public endpoints |
| **PS.4** Protect Code & Configuration | Injection, Security Misconfiguration | T1059 Command and Scripting Interpreter, T1190 | — | Strong input validation, parameterized queries, and least privilege in code |
| **PW.1** Secure Coding Practices | Injection, Broken Access Control, BOLA | T1059, T1190, T1552 | — | Code review checklist explicitly mapped to OWASP risks + SAST rules |
| **PW.2** Review & Test Code | All OWASP risks | — | — | Threat-informed test cases (including abuse cases derived from ATT&CK) + DAST + IAST |
| **PW.3** Manage Technical Debt | All risks (especially recurring ones) | — | NVD trending CVEs + exploit activity | Backlog prioritized by CVSS + EPSS + business impact + active exploitation |
| **RV.1** Respond to Vulnerabilities | All risks | — | NVD + MISP + Talos | Automated patching pipeline with SLAs based on severity and threat intelligence |
| **RV.2** Respond to Incidents | All risks | All techniques (post-incident learning) | FIRST + SANS ISC + internal telemetry | Structured logging (STIX-ready) + incident playbooks + feedback loop into threat modeling |

---

## Detailed Example: BOLA (Broken Object Level Authorization)

**Risk**: OWASP API Security Risk #1 (2023)  
**Description**: Attacker manipulates object identifiers in requests to access or modify resources belonging to other users.

### SSDF Practices That Mitigate BOLA

| SSDF Practice | How It Helps |
|---------------|--------------|
| **PO.1** Define Security Requirements | Explicitly require object-level authorization checks for every endpoint that accesses resources by ID |
| **PW.1** Secure Coding Practices | Always validate that `authenticated_user.id == requested_object.owner_id` (or use a policy engine) |
| **PW.2** Review & Test Code | Implement automated tests using different user contexts and property-based testing |

### Relevant MITRE ATT&CK Techniques

- **T1078 Valid Accounts** — Attacker uses legitimate (but low-privilege) credentials
- **T1190 Exploit Public-Facing Application** — Abuses exposed API endpoints
- **T1134 Access Token Manipulation** — Modifies claims or tokens to escalate access

### Threat Intelligence Input

- Monitor MISP and Talos for active BOLA/IDOR exploitation campaigns targeting similar technologies or industries
- Review breach reports (Verizon DBIR, Have I Been Pwned, public post-mortems) to understand real-world impact

### Implementation Recommendation

See the `examples/fastapi-threat-shield/` directory for a practical middleware + decorator pattern that enforces object-level authorization.

---

## How to Extend and Customize This Mapping

This table is a starting point. In a real organization, you should:

1. **Customize ATT&CK techniques** to your specific technology stack, cloud providers, and industry.
2. **Add compliance columns** (LGPD, GDPR, PCI-DSS, ISO 27001, SOC 2, etc.) to demonstrate how SSDF practices also support regulatory requirements.
3. **Link to internal artifacts** — Connect each row to your threat models, policies, code examples, and backlog items.
4. **Evolve it over time** — Update the mapping as new ATT&CK techniques emerge and as your threat landscape changes.

---

## Conclusion

The real power of this repository lies in **integration**, not in any single framework.

OWASP tells us *what* is dangerous.  
MITRE ATT&CK tells us *how* attackers actually operate.  
NIST SSDF tells us *when* in the lifecycle we should act.  
Threat Intelligence tells us *what is happening right now*.

This mapping document turns these four perspectives into a single, actionable view. It helps teams stop treating security as a separate activity and instead embed it into everyday decisions about architecture, coding, testing, and operations.

