# OWASP Risks → Concrete Controls & Mitigations

This document maps the most critical OWASP risks (from both the **Web Top 10 2025** and **API Security Risks 2023**) to concrete, actionable controls. It serves as a practical reference for developers and security engineers who want to move beyond checklists and implement real defenses.

The goal is to provide **preventive, detective, and responsive** controls for each risk, while connecting them to threat intelligence and the broader **Threat-Informed Secure Development** framework.

---

## How to Use This Document

For every major OWASP risk, this document answers four practical questions:

1. **How do I prevent this at design and development time?**
2. **How do I detect it in code, pipeline, or runtime?**
3. **How do I block or mitigate it when threat intelligence indicates active exploitation?**
4. **How do I respond and improve continuously?**

The best teams address all four layers.

---

## Top Priority Risks (2025–2026)

### 1. Broken Access Control / BOLA (API Security Risk #1)

**Why it remains the #1 risk**  
Broken Object Level Authorization (BOLA/IDOR) continues to be the most exploited API vulnerability. Modern frameworks make it easy to expose object identifiers, but developers often forget to validate ownership or permissions on every request.

**Concrete Controls**

| Layer          | Control                                                                 | Implementation Example                          |
|----------------|-------------------------------------------------------------------------|-------------------------------------------------|
| **Preventive** | Server-side object-level authorization on every endpoint                | Middleware/Decorator comparing `user.id` with `resource.owner_id` or using a policy engine (OPA/Cedar) |
| **Preventive** | Use non-sequential, unpredictable identifiers (UUIDv4)                  | Replace auto-increment IDs with UUIDs           |
| **Detective**  | Logging and alerting on anomalous access patterns                       | Detect when one user accesses thousands of objects in a short time |
| **Responsive** | Rate limiting + temporary blocking on suspicious authorization failures | Integrate with `FastAPI Threat Shield`          |

**Threat Intelligence Connection**  
Monitor MISP and sector-specific feeds for active BOLA/IDOR exploitation campaigns. MITRE ATT&CK techniques **T1078 (Valid Accounts)** and **T1134 (Access Token Manipulation)** are frequently used together with BOLA.

**Recommended Testing**  
- Property-based testing with different user roles and contexts  
- Automated tests attempting to access another user’s resources using valid tokens

---

### 2. Security Misconfiguration (API Security Risk #8)

**Why it is critical**  
Default configurations, exposed debug endpoints, overly permissive CORS, missing security headers, and cloud misconfigurations remain extremely common and easy to exploit.

**Concrete Controls**

| Layer          | Control                                      | Recommended Tools / Practices                     |
|----------------|----------------------------------------------|---------------------------------------------------|
| **Preventive** | Infrastructure as Code with security baselines | CIS Benchmarks + Terraform / Pulumi policies      |
| **Preventive** | API Gateway + WAF with updated rule sets     | AWS WAF, Cloudflare, or Kong with OWASP CRS       |
| **Preventive** | Disable debug mode and remove sensitive headers in production | `X-Powered-By`, stack traces, version headers     |
| **Detective**  | Continuous configuration scanning            | Trivy config scanning, Checkov, OPA/Gatekeeper    |
| **Responsive** | Automated drift detection and remediation    | Integrate with SIEM or response playbooks         |

**Course Insight (Daniel Lemeszenski)**  
The principle of **“Menos é Mais”** (Less is More) directly applies here: minimize exposed information and permissions. The fewer details an attacker can gather from error messages or headers, the harder it becomes to exploit misconfigurations.

---

### 3. Software Supply Chain Failures (OWASP Web Top 10 #3 - 2025)

**Why it gained priority**  
Supply chain attacks (SolarWinds, XZ Utils, Codecov, etc.) have shown that compromising a single dependency can affect thousands of organizations.

**Concrete Controls**

| Layer          | Control                                           | Tools                                      |
|----------------|---------------------------------------------------|--------------------------------------------|
| **Preventive** | SBOM generation + signing for every release       | Syft + Cosign                              |
| **Preventive** | Dependency pinning + automated update management  | Dependabot + Renovate                      |
| **Detective**  | CVE scanning with exploitability context          | Grype + EPSS + NVD                         |
| **Responsive** | Pipeline gate: block deployment on Critical/High CVEs with known active exploitation | Custom GitHub Action or GitLab CI rule     |

**Threat Intelligence Connection**  
Use NVD + EPSS scores combined with MISP or Talos feeds to understand whether a CVE is being actively exploited in the wild, rather than blocking every high CVSS score blindly.

---

### 4. Broken Authentication

**Why it matters**  
Weak authentication mechanisms, long-lived tokens, lack of re-authentication for sensitive operations, and custom authentication implementations are still major sources of account takeover.

**Concrete Controls**

| Layer          | Control                                              | Recommendation                                      |
|----------------|------------------------------------------------------|-----------------------------------------------------|
| **Preventive** | Use mature, battle-tested Identity Providers         | Auth0, Azure AD (Entra ID), AWS Cognito, Keycloak   |
| **Preventive** | Short-lived access tokens + refresh token rotation   | Implement OAuth 2.0 + OIDC properly                 |
| **Preventive** | Re-authentication for high-risk operations           | Changing email, PIX key, password, or large transfers |
| **Detective**  | Monitor for credential stuffing and anomalous logins | Integrate with threat intel feeds (e.g., leaked credentials) |

**Strong Recommendation**  
Avoid building custom authentication systems from scratch unless you have deep expertise and dedicated security review capacity. Use established Identity Providers instead.

---

### 5. Improper Inventory Management (API Security Risk #9)

**Why it is dangerous**  
Organizations lose track of APIs (especially shadow, partner, and deprecated APIs). This leads to unpatched systems, forgotten endpoints with sensitive data, and increased attack surface.

**Concrete Controls**

| Layer          | Control                                           | Implementation                              |
|----------------|---------------------------------------------------|---------------------------------------------|
| **Preventive** | Maintain a complete and up-to-date API inventory  | API Gateway + service catalog (Backstage, etc.) |
| **Preventive** | Enforce versioning and deprecation policies       | Clear lifecycle for every API version       |
| **Preventive** | Documentation as code                             | OpenAPI specs published automatically       |
| **Detective**  | Regular discovery scans of environments           | Automated detection of undocumented endpoints |

---

## The 4-Layer Defense Model

For every OWASP risk, apply this structured approach:

| Layer       | Question                                              | Examples                                      | Owner          |
|-------------|-------------------------------------------------------|-----------------------------------------------|----------------|
| **Prevent** | Can I stop this at design or development time?        | Threat modeling, secure design patterns, policy-as-code | Developers + Architects |
| **Detect**  | Can I find it in code, pipeline, or runtime?          | SAST, SCA, config scanning, runtime monitoring   | DevSecOps + Security |
| **Block**   | Can I stop exploitation when threat intelligence indicates active campaigns? | WAF rules, middleware (FastAPI Threat Shield), rate limiting | Security Engineering |
| **Respond** | Can I investigate, contain, and learn from incidents? | Structured logging, incident playbooks, STIX export | Security + SRE |

The strongest teams implement controls across **all four layers**.

---

## Conclusion

OWASP lists provide an excellent prioritization framework, but they are not enough on their own. Real security comes from translating these risks into **concrete, layered controls** that are enforced throughout the development lifecycle.

This document shows that effective mitigation requires more than just “following OWASP.” It demands:

- **Threat-informed prioritization** — using MITRE ATT&CK and active threat intelligence to focus on what attackers are actually doing right now.
- **Defense in depth** — combining preventive design, automated detection in pipelines, runtime protection, and structured response.
- **Practical implementation** — moving from theory to code patterns, tests, and automation that developers can actually use.

When combined with the **NIST SSDF** lifecycle and structured threat intelligence (STIX/MISP), these controls become part of a living, threat-informed secure development practice — not a static checklist.


