# Threat Intelligence Sources → NIST SSDF Practices

This mapping shows **where and how** to inject real threat intelligence into your secure development process.

## Summary Table

| Threat Intelligence Source | Type of Data | Best SSDF Phase(s) | How to Use in Development | Automation Level |
|----------------------------|--------------|--------------------|---------------------------|------------------|
| **NVD (NIST)** + CVE/CWE   | Known vulnerabilities + CVSS + affected products | PS.2 (3rd Party Components), RV.1 (Vuln Response) | Dependency scanning gate in CI/CD. Block Critical/High new CVEs. Map to CWE/OWASP. | High (API + tools like Trivy, Grype, Dependabot) |
| **MITRE ATT&CK**           | TTPs (Tactics, Techniques, Procedures) | PO.1 (Requirements), PO.2 (Risk Assessment), PW.2 (Testing) | Threat modeling workshops. Design abuse cases. Write tests that simulate attacker techniques. | Medium (use ATT&CK Navigator + custom matrices) |
| **MISP** (public + private instances) | IOCs (IPs, domains, hashes, URLs, filenames) + TTPs | PS.3 (Build/Deploy), RV.2 (Incident Response), Runtime protection | API middleware that queries MISP for reputation. Block or challenge suspicious requests. Export your own findings in STIX. | High (pymisp + FastAPI middleware) |
| **Cisco Talos**            | Research, Snort/Suricata rules, reputation, malware intel | PS.3, RV.2, Runtime | Feed Talos reputation into WAF/API Gateway. Use Snort rules for detection. Read their reports for emerging threats. | Medium-High |
| **SANS ISC / DShield**     | Internet Storm Center, malicious IP data | Runtime protection, monitoring | Simple IP reputation checks in middleware or edge. | High |
| **AbuseIPDB / AlienVault OTX** | IP/Domain/URL reputation | Runtime protection | Fast, free reputation API for blocking in API layer. | High |
| **FIRST**                  | Incident response coordination, best practices | RV.2 (Incident Response) | Improve logging and incident playbooks. Learn from real coordinated responses. | Low-Medium |
| **CIS Benchmarks**         | Hardening guides (prescriptive) | PS.3 (Secure Build & Deploy) | Use as baseline for Docker, Kubernetes, Linux, cloud configs. Automate compliance checks. | High (InSpec, CIS-CAT, etc.) |

## Recommended Starting Point for Most Teams

1. **NVD integration in CI/CD** (highest ROI, lowest effort)
2. **MISP or AbuseIPDB in API middleware** (real-time protection)
3. **ATT&CK-informed threat modeling** (improves design quality dramatically)
4. **CIS Benchmarks** for infrastructure-as-code hardening

This combination already puts you ahead of most development teams in 2026.
