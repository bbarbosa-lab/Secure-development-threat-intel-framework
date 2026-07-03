# 03 - Secure Lifecycle with NIST SSDF v1.1

The **NIST Secure Software Development Framework (SSDF)** v1.1 (SP 800-218) is currently the most complete and authoritative framework for structuring secure development in an organization.

It organizes practices into four major phases:

## 1. Prepare the Organization (PO)

- Define security policies, requirements, and culture
- Train teams
- Continuously identify and assess risks
- **Where threat intelligence enters strongly**: PO.2 (Identify and Assess Risks) — use ATT&CK + current feeds to prioritize real risks, not generic ones.

## 2. Protect the Software (PS)

- Securely manage third-party components
- Protect sensitive data (encryption + key management)
- Secure configuration of builds and deployments
- Protect the source code itself
- **Where threat intelligence enters**: PS.2 (3rd Party) via NVD + SCA tools. PS.3 via CIS Benchmarks + threat-informed hardening.

## 3. Produce Well-Secured Software (PW)

- Secure coding practices
- Code review + security testing (SAST, DAST, IAST)
- Manage security technical debt
- **Where threat intelligence enters**: PW.2 — write tests based on real ATT&CK techniques (abuse cases).

## 4. Respond to Vulnerabilities (RV)

- Vulnerability disclosure and response process
- Security incident response
- Lessons learned and feedback into the process
- **Where threat intelligence enters strongly**: RV.1 and RV.2 — use feeds to prioritize patching and improve detection.

## Why SSDF is Better Than "Just Following OWASP Top 10"?

OWASP Top 10 is excellent for **prioritizing risks**.

SSDF is excellent for **structuring the process** that prevents, detects, and fixes those risks sustainably.

The ideal approach is to use both together (as we do in this repository).

## Practical Recommendation

Start by implementing the **Prepare** and **Protect** practices (especially dependency management and secure configuration). Then move to **Produce** with threat-informed reviews and testing. Finally, mature the **Respond** phase.

When executed well, this cycle dramatically reduces security cost and risk over time.
