# Secure-Development-Threat-Intel-Framework

> **Building secure software by design, informed by real threats, standards, and intelligence.**

A practical framework that integrates **NIST SSDF**, **OWASP Top 10 (Web + API)**, **MITRE ATT&CK**, **CVE/CWE + NVD**, and structured threat intelligence (**STIX/TAXII + MISP + Talos**) into the entire software development lifecycle.

This repository demonstrates how to move from reactive security to **proactive, threat-informed secure development** — exactly the mindset expected for AppSec, DevSecOps, and Product Security professionals in 2026.

---

## 🎯 The Core Philosophy (Mindset)

Security is not a checklist you add at the end.  
Security is a **continuous decision-making process** that starts at the whiteboard and lives in every line of code, every configuration, and every deployment.

The best way to build secure systems is to **understand how attackers actually think and operate** (MITRE ATT&CK + real threat intelligence) and then systematically remove the conditions that allow those techniques to succeed (OWASP + SSDF + secure coding standards).

This framework treats **threat intelligence as a first-class input** to every phase of development — not just something the SOC consumes.

---

## 🧩 Frameworks That Actually Matter (2026)

| Framework                        | Role in This Framework                          | Why It Matters for Developers                  | Priority |
|----------------------------------|------------------------------------------------|------------------------------------------------|----------|
| **NIST SSDF v1.1** (SP 800-218) | The backbone lifecycle (Prepare → Protect → Produce → Respond) | Provides the "how to structure secure development" at organizational level | ★★★★★ |
| **OWASP Top 10 2025** + **OWASP Top 10 API 2023** | The prioritized risk list for web apps and APIs | Tells you exactly what to hunt for in code and design | ★★★★★ |
| **MITRE ATT&CK**                 | Real attacker tactics, techniques & procedures | Turns vague "threats" into concrete defensive requirements | ★★★★★ |
| **MITRE CVE + CWE + NVD**        | Known vulnerabilities and root weaknesses      | Objective way to measure and prioritize technical debt | ★★★★★ |
| **STIX 2.1 + TAXII 2.1 + MISP**  | Machine-readable threat intelligence exchange  | Enables automation and sharing of IOCs/TTPs    | ★★★★☆ |
| **CIS Benchmarks**               | Prescriptive hardening standards               | Concrete configuration guidance for OS, containers, cloud | ★★★★☆ |

---

##  The Integrated Model

This repository presents a **Threat-Informed Secure Development Lifecycle** that maps:

- NIST SSDF practices  
- OWASP risks (Web + API)  
- MITRE ATT&CK techniques relevant to developers  
- Concrete threat intelligence sources and how to consume them in code/CI/CD

See the full mapping in [`mappings/ssdf-owasp-attck-mapping.md`](./mappings/ssdf-owasp-attck-mapping.md)

---

##  Repository Structure

```
threat-informed-secure-development/
├── README.md
├── LICENSE
├── CONTRIBUTING.md
├── docs/
│   ├── 01-introduction-and-mindset.md
│   ├── 02-core-frameworks.md
│   ├── 03-secure-development-lifecycle-ssdf.md
│   ├── 04-owasp-top-10-2025.md
│   ├── 05-mitre-attck-for-developers.md
│   ├── 06-cve-cwe-nvd-practical-use.md
│   └── 07-integrated-secure-development-model.md
├── mappings/                               # ← Highest value content
│   ├── ssdf-owasp-attck-mapping.md
│   ├── threat-intel-to-ssdf.md
│   └── owasp-risks-to-controls.md
├── diagrams/
│   ├── secure-development-lifecycle.png
│   ├── threat-informed-decision-flow.png
│   └── ssdf-owasp-attck-mapping.png
├── references/
│   └── references-and-sources.md

```


---

##  Practical Examples Included

### 1. FastAPI Threat Shield (Main Example)
A production-ready middleware that:
- Checks incoming requests against threat intelligence feeds (AbuseIPDB + local MISP)
- Blocks or challenges known malicious IPs/domains
- Logs security events in structured format (ready for STIX export)
- Demonstrates defense-in-depth at the API edge

### 2. NVD Dependency Check
Automated CVE scanning in CI/CD pipeline with severity gating.

### 3. STIX/MISP Consumer
Example of consuming structured threat intelligence and converting it into actionable controls.

---

## 📚 Documentation (Deep Dives)

- [01 - Introduction and Mindset](./docs/01-introduction-and-mindset.md)
- [02 - Core Frameworks](./docs/02-core-frameworks.md)
- [03 - Secure Development Lifecycle with NIST SSDF](./docs/03-secure-development-lifecycle-ssdf.md)
- [04 - OWASP Top 10 2025 + API Risks 2023](./docs/04-owasp-top-10-2025.md)
- [05 - MITRE ATT&CK for Developers](./docs/05-mitre-attck-for-developers.md)
- [06 - CVE and CVW practical use](./docs/06-cve-cwe-nvd-practical-use.md)
- [07 - Integrated Secure Development Model](./docs/07-integrated-secure-development-model.md)

---

##  Key Mappings (The Real Value)

These tables are the heart of this repository:

- **SSDF × OWASP × ATT&CK Mapping** → How each SSDF practice helps mitigate specific OWASP risks and ATT&CK techniques
- **Threat Intelligence Sources → SSDF Practices** → Where to inject real threat data into your process
- **OWASP Risks → Concrete Controls** → What to actually implement in code and pipelines

---

##  About This Work

This framework to consolidate my studies about cyber threat intelligence and secure development.

- Understanding of modern secure development standards
- Ability to synthesize multiple authoritative sources (NIST, OWASP, MITRE, STIX) into a coherent, actionable model
- Practical implementation skills (Python/FastAPI examples)
- Threat-informed thinking — the ability to connect what attackers do with what developers must build

It reflects real learning from postgraduate studies in **Red Team Operations & Offensive Cyber** and Cisco's course **Cyber Threat Management** combined with hands-on infrastructure and development experience.

---

**If you are a recruiter or hiring manager**: This repository shows how I think about security as a developer.

---

*Create by Bruno Barbosa*
*Last updated: July 2026*
