# 07 - Integrated Secure Development Model

**The Threat-Informed Secure Development Framework**

This is the final and most important document in this repository. It synthesizes everything we have discussed into one cohesive, actionable model that you can apply in real projects.

---

## Why Integration Matters

Most organizations treat secure development and threat intelligence as two separate worlds:

- **Secure Development** teams focus on OWASP Top 10, code reviews, SAST/DAST, and compliance checklists.
- **Threat Intelligence** teams focus on IOCs, TTPs, CVEs, and monitoring the external threat landscape.

This separation creates a critical gap: **security decisions are made without real context about how attackers are actually operating today**.

The **Integrated Secure Development Model** closes this gap. It makes threat intelligence a first-class citizen in every phase of the software development lifecycle — from architecture decisions to runtime protection.

---

## Core Philosophy

This model is built on three foundational principles:

| Principle                    | Description                                                                 | Practical Meaning |
|-----------------------------|-----------------------------------------------------------------------------|-------------------|
| **Threat-Informed**         | Every security decision is guided by real attacker behavior (MITRE ATT&CK) and current threat data | We don't just follow checklists — we ask *"How would a real attacker exploit this right now?"* |
| **Secure by Design**        | Security is built into the architecture from day one, not bolted on later   | We design controls before writing the first line of code |
| **Defense in Depth**        | Multiple overlapping layers of protection so that no single failure compromises the system | Even if one control fails (e.g., a misconfigured WAF), other layers still protect the application |

This approach directly reflects concepts taught in professional courses such as **Cyber Threat Management** and **Secure Development**, where *Defense in Depth* and proper governance of APIs are emphasized as non-negotiable foundations.

---

## The Integrated Lifecycle (Threat-Informed SSDF)

We use **NIST SSDF v1.1** as the structural backbone because it is the most comprehensive and widely respected secure development lifecycle framework. We then enrich every phase with threat intelligence inputs and OWASP/ATT&CK guidance.

### Visual Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                    THREAT-INFORMED SECURE DEVELOPMENT                │
├──────────────┬──────────────┬──────────────┬──────────────┬─────────┤
│   PREPARE    │    DESIGN    │   DEVELOP    │   DEPLOY     │ RESPOND │
│   (Govern)   │  (Threat     │   (Build)    │  (Runtime)   │         │
│              │   Modeling)  │              │              │         │
├──────────────┼──────────────┼──────────────┼──────────────┼─────────┤
│ • Risk       │ • ATT&CK     │ • NVD/CVE    │ • FastAPI    │ • STIX  │
│   Appetite   │   Techniques │   Checks     │   Threat     │   Logs  │
│ • SSDF PO    │ • OWASP      │ • SAST/SCA   │   Shield     │ • MISP  │
│ • Policy     │   Mapping    │ • Secure     │ • WAF +      │   IOCs  │
│              │ • Data Flow  │   Coding     │   Reputation │ • IR    │
│              │   Diagrams   │   Standards  │   Feeds      │   Play- │
│              │              │              │              │   books │
└──────────────┴──────────────┴──────────────┴──────────────┴─────────┘
                              ↑
                    Continuous Threat Intelligence Feedback Loop
```

---

## Phase-by-Phase Integration

### 1. Prepare / Govern (SSDF PO - Prepare the Organization)

**Goal:** Establish the foundation and risk appetite.

**Threat Intelligence Inputs:**
- High-level threat landscape reports (SANS @RISK, Talos, CIS)
- Regulatory requirements and industry-specific threats
- Lessons learned from past incidents (FIRST, internal IR)

**Key Actions:**
- Define which **OWASP Top 10 API Risks** (2023) are most relevant to your business (e.g., BOLA is almost always #1 for APIs).
- Establish a **Threat Modeling** standard using MITRE ATT&CK.
- Decide on your **Security Misconfiguration** tolerance (from the course material: "quanto menos informação e permissão você conceder, mais seguro será o seu ecossistema").

### 2. Design (Threat Modeling + Architecture)

**Goal:** Make security design decisions based on real threats.

**Threat Intelligence Inputs:**
- Relevant **MITRE ATT&CK techniques** for your technology stack and exposure (T1190 - Exploit Public-Facing Application, T1552 - Unsecured Credentials, etc.).
- Known exploitation patterns for your frameworks (from NVD + Exploit-DB).
- Business logic abuse patterns (Unrestricted Access to Sensitive Business Flows).

**Key Actions:**
- Create **Data Flow Diagrams** and threat model every new feature.
- Apply **"Menos é Mais"** principle (from API governance material): minimize exposed endpoints, HTTP verbs, and returned data.
- Design **Broken Object Level Authorization (BOLA)** prevention from day one (use unpredictable IDs like GUIDs + policy-based authorization checks).

### 3. Develop & Build

**Goal:** Write secure code and catch issues early.

**Threat Intelligence Inputs:**
- Fresh **CVE data** via NVD API (block builds with Critical/High CVEs).
- Known vulnerable components and supply chain risks.
- Common weakness patterns (CWE) mapped to OWASP.

**Key Actions:**
- Implement **secure coding standards** aligned with OWASP ASVS.
- Use **SAST + SCA** tools (Semgrep, Trivy, Snyk).
- Apply **double custody** thinking (from mobile/API courses): both backend and frontend must validate and protect sensitive operations.
- Enforce re-authentication for sensitive actions (as emphasized in authentication best practices).

### 4. Deploy & Runtime Protection

**Goal:** Protect the application where it is most exposed.

**Threat Intelligence Inputs:**
- Real-time **IOC feeds** (malicious IPs, domains, user-agents).
- Reputation data (AbuseIPDB, OTX, Talos, your own MISP instance).
- Emerging attack patterns observed in the wild.

**Key Actions:**
- Deploy the **FastAPI Threat Shield** (or equivalent) at the edge.
- Enforce **proper inventory management** (OWASP API Risk #9): know every public, internal, and partner API.
- Apply **Security Misconfiguration** hardening (CIS Benchmarks + principle of least privilege).
- Implement **certificate pinning** and strong TLS practices (from network protection material).

### 5. Monitor, Respond & Improve

**Goal:** Learn from reality and continuously improve.

**Threat Intelligence Inputs:**
- Security events logged in **structured format** (ready for STIX export).
- New IOCs and TTPs from MISP, Talos, and internal detection.
- Post-incident reviews mapped back to ATT&CK techniques.

**Key Actions:**
- Export security logs in a format that can be shared via **STIX/TAXII**.
- Feed findings back into threat models and secure coding standards.
- Continuously update your **Threat-Informed Decision Flow**.

---

## Connecting Threat Intelligence to Daily Decisions

One of the most practical outputs of this model is the **Threat-Informed Decision Flow**:

```
Incoming Request / New Feature
          ↓
Is this related to a known ATT&CK technique or recent CVE?
          ↓
Check against current threat intelligence (MISP / NVD / Reputation)
          ↓
Apply relevant OWASP control + least privilege ("menos é mais")
          ↓
Log the decision in structured format (STIX-ready)
          ↓
Feed outcome back into threat models and policies
```

This loop turns threat intelligence from a passive report into an active control that influences architecture, code, and runtime behavior.

---

## Practical Examples in This Repository

This repository provides concrete implementations of the model:

| Example                        | How It Demonstrates the Integrated Model                          | Recommended Starting Point |
|--------------------------------|-------------------------------------------------------------------|----------------------------|
| **FastAPI Threat Shield**      | Runtime protection layer using threat intelligence + structured logging | **Start here** |
| **NVD Dependency Check**       | Build-phase integration of CVE intelligence                       | High value for CI/CD |
| **STIX/MISP Consumer**         | Consuming structured threat intelligence and turning it into controls | Shows automation potential |

---

## How to Adopt This Model (Pragmatic Path)

You do **not** need to implement everything at once. A realistic adoption path is:

1. **Week 1-2**: Start threat modeling new features using MITRE ATT&CK + OWASP mapping.
2. **Week 3-4**: Add NVD CVE checking in your CI/CD pipeline (use the NVD example).
3. **Month 2**: Deploy a basic version of the FastAPI Threat Shield (or equivalent middleware) for IP/domain reputation.
4. **Month 3+**: Introduce structured logging (STIX-ready) and connect it to your internal MISP or SIEM.
5. **Ongoing**: Review the `mappings/` folder regularly and update your threat models based on new intelligence.

---

## Final Thoughts

The **Integrated Secure Development Model** is not a rigid methodology. It is a **way of thinking**:

> Security is not a phase.  
> It is a continuous conversation between what attackers are doing in the real world and what we build in our code.

By combining:
- **NIST SSDF** (process)
- **OWASP Top 10 + API Risks** (what to protect against)
- **MITRE ATT&CK** (how attackers actually operate)
- **Structured Threat Intelligence** (STIX/MISP/Talos/NVD) (real-time context)

…you move from reactive security to **threat-informed, proactive secure development**.

This is exactly the mindset that senior AppSec engineers, Product Security teams, and modern DevSecOps professionals are expected to have in 2026.

---

## References & Further Reading

- NIST SP 800-218 – Secure Software Development Framework (SSDF)
- OWASP Top 10:2025 and OWASP Top 10 API Security Risks 2023
- MITRE ATT&CK Framework
- STIX 2.1 / TAXII 2.1 Specifications + MISP Project
- Course material: *Cyber Threat Management* and *Secure Development* (Daniel Lemeszenski) — especially topics on Defense in Depth, BOLA prevention, Security Misconfiguration, and API governance ("menos é mais").

---

**This document closes the repository.**  
Everything else in this project exists to support the practical application of this integrated model.

If you internalize and apply what is described here, you will be building software that is not only "secure by checklist" but genuinely **resilient against real threats**. 

That is the difference between someone who follows security practices and someone who *thinks* in secure development.