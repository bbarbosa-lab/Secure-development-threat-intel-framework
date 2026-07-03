# Threat Intelligence Sources → NIST SSDF Practices

This mapping shows **where and how** to systematically inject real threat intelligence into your secure development lifecycle. It connects external threat data sources directly to specific NIST SSDF practices, turning threat intelligence from a passive activity into an active driver of secure design, development, and response.

---

## Why This Mapping Matters

Most development teams consume threat intelligence in a fragmented and reactive way — reading reports occasionally or copying IOC lists manually. This approach creates little lasting value.

Mature teams do the opposite: they **embed threat intelligence into the development process itself**. They use it to:

- Prioritize what to build securely
- Detect real attacks in their pipelines and runtime
- Make informed trade-off decisions under time pressure
- Continuously improve based on what attackers are actually doing

This document provides a clear blueprint for achieving that integration.

---

## How to Use This Mapping

| When you are...                        | Use this mapping to...                                                                 | Outcome |
|----------------------------------------|----------------------------------------------------------------------------------------|---------|
| Starting a new project or feature      | Identify which threat intel sources should influence your threat models and security requirements | Threat models grounded in current attacker behavior |
| Designing CI/CD pipelines              | Decide which automated checks (NVD, SCA, config scanning) to implement and how strict the gates should be | Stronger, risk-based security gates |
| Building runtime protection            | Choose which sources (MISP, AbuseIPDB, Talos) to query in your API middleware or WAF | Real-time, threat-informed blocking |
| Responding to incidents                | Define what data to log and how to structure it for correlation with external threat intel | Faster investigation and better feedback loops |
| Prioritizing security work             | Justify investment by linking controls to active threats and SSDF practices            | Better communication with leadership |

---

## Summary Mapping Table

| Threat Intelligence Source          | Type of Data                              | Best SSDF Phase(s)                          | How to Use in Development                                                                 | Automation Level      | Recommended Starting Action |
|-------------------------------------|-------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------|-----------------------|-----------------------------|
| **NVD (NIST) + CVE/CWE**            | Known vulnerabilities, CVSS, affected products, CWE mappings | PS.2, RV.1                                 | Dependency scanning in CI/CD. Block builds on Critical/High CVEs with known exploits. Map findings to OWASP risks. | High                  | Integrate Grype/Trivy + Dependabot in every pipeline |
| **MITRE ATT&CK**                    | Tactics, Techniques, and Procedures (TTPs) | PO.1, PO.2, PW.2                           | Run threat modeling workshops using ATT&CK Navigator. Create abuse cases and test scenarios based on real techniques. | Medium                | Filter ATT&CK Navigator by your technology stack and industry |
| **MISP** (public or private)        | IOCs (IPs, domains, hashes, URLs) + TTPs  | PS.3, RV.2, Runtime protection             | Query MISP from API middleware to block/challenge suspicious requests. Export your own security events in STIX format. | High                  | Start with a public MISP instance + `pymisp` library |
| **Cisco Talos**                     | Research reports, reputation data, Snort rules, malware intel | PS.3, RV.2, Runtime                        | Feed reputation data into WAF/API Gateway. Use research reports to anticipate emerging threats. | Medium-High           | Subscribe to Talos blog + integrate reputation where available |
| **SANS ISC / DShield**              | Early warning signals, malicious IP data  | Runtime protection, monitoring             | Simple IP reputation checks at the edge or in middleware for early blocking.              | High                  | Add lightweight IP reputation check in API gateway |
| **AbuseIPDB / AlienVault OTX**      | IP, domain, and URL reputation            | Runtime protection                         | Fast, free reputation checks directly in application middleware or edge layer.            | High                  | Implement in `FastAPI Threat Shield` or equivalent |
| **FIRST**                           | Incident response coordination and best practices | RV.2                                       | Improve logging standards and incident response playbooks using real coordinated response lessons. | Low-Medium            | Review FIRST resources when building incident playbooks |
| **CIS Benchmarks**                  | Prescriptive hardening guidelines         | PS.3                                       | Use as the baseline for Docker, Kubernetes, Linux, cloud, and database configurations. Automate compliance checks. | High                  | Run CIS-CAT or InSpec against infrastructure-as-code |

---

## High-Value Implementation Examples

### Example 1: NVD Integration in CI/CD (Highest ROI)

**SSDF Practices**: PS.2 and RV.1  
**Goal**: Prevent vulnerable dependencies from reaching production.

**Implementation**:
- Run `grype` or `trivy` on every build
- Fail the pipeline if a **Critical** or **High** CVE with known exploit (EPSS > threshold) is found
- Map findings to CWE and OWASP categories for better reporting

This single automation significantly reduces your software supply chain risk.

### Example 2: MISP + API Middleware (Real-time Protection)

**SSDF Practices**: PS.3 and Runtime protection  
**Goal**: Block or challenge requests from known malicious infrastructure.

**Implementation**:
- Use `pymisp` to query a MISP instance for IP/domain reputation
- Create a FastAPI middleware that checks incoming requests
- Log security events in a structured format (ready for future STIX export)

See `examples/fastapi-threat-shield/` for a reference implementation pattern.

### Example 3: ATT&CK-Informed Threat Modeling

**SSDF Practices**: PO.1 and PO.2  
**Goal**: Make threat modeling concrete instead of theoretical.

**Implementation**:
- Before designing a new endpoint or feature, filter ATT&CK techniques relevant to your technology (e.g., T1190, T1078, T1552 for web APIs)
- Ask: “How would an attacker apply this technique against this specific functionality?”
- Document the answers and derive security requirements and test cases

This dramatically improves the quality of threat models.

---

## Recommended Implementation Roadmap

| Phase | Focus Area                          | Key Actions                                                                 | Expected Time | Business Value |
|-------|-------------------------------------|-----------------------------------------------------------------------------|---------------|----------------|
| **1** | Foundation                          | Integrate NVD/CVE scanning into all CI/CD pipelines                         | 1–2 weeks     | High           |
| **2** | Runtime Protection                  | Add IP/domain reputation checks (AbuseIPDB or MISP) in API middleware       | 2–3 weeks     | High           |
| **3** | Design Quality                      | Introduce ATT&CK-informed threat modeling for new features                  | 3–4 weeks     | Very High      |
| **4** | Infrastructure Hardening            | Adopt CIS Benchmarks as baseline for containers and cloud infrastructure    | 4–6 weeks     | High           |
| **5** | Maturity & Feedback                 | Structured logging + export security events to MISP + close the loop with threat modeling | 2–3 months    | Very High      |

Even completing phases 1 and 2 already puts most development teams ahead of the majority in 2026.

---

## Conclusion

Threat intelligence only creates real value when it influences decisions **inside** the development lifecycle — not when it remains a separate activity performed by the security team.

This mapping demonstrates exactly how to achieve that integration:

- **NVD** protects your software supply chain
- **ATT&CK** improves the quality of your designs and tests
- **MISP, Talos, AbuseIPDB** enable real-time, threat-informed runtime protection
- **CIS Benchmarks** raise the baseline security of your infrastructure

When combined with the **NIST SSDF** structure and the risk prioritization provided by **OWASP**, threat intelligence stops being noise and becomes a strategic advantage.
