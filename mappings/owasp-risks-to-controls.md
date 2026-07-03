# OWASP Risks → Concrete Controls & Mitigations

This document provides direct, actionable controls for the most important OWASP risks (Web Top 10 2025 + API Security Risks 2023).

## Top Priority Risks (2025/2026)

### 1. Broken Access Control / BOLA (API #1)
**Control**: Object-level authorization check on every request that accesses a resource by ID.
**Code Pattern**: Decorator or middleware that compares `authenticated_user.id` with `resource.owner_id` (or uses policy engine).
**Testing**: Property-based tests + tests with different user roles/contexts.
**Threat Intel**: Watch for campaigns abusing IDOR/BOLA in your sector.

### 2. Security Misconfiguration (API #8)
**Control**: 
- Infrastructure as Code with CIS Benchmarks
- API Gateway + WAF rules
- No debug endpoints in production
- Proper CORS, TLS, headers
**Automation**: Trivy config scanning + OPA/Gatekeeper policies in Kubernetes.

### 3. Software Supply Chain Failures (OWASP Web #3 2025)
**Control**:
- SBOM generation + signing
- Dependency pinning + automated updates (Dependabot + Renovate)
- CVE gate in pipeline (block on Critical/High with known exploit)
**Tooling**: Syft + Grype + Cosign

### 4. Broken Authentication
**Control**:
- Use mature Identity Provider (Auth0, Azure AD, AWS Cognito, Keycloak)
- Short-lived tokens + refresh rotation
- Re-authentication for sensitive operations
- MFA where appropriate
**Never**: Custom auth from scratch unless you have deep expertise.

### 5. Improper Inventory Management (API #9)
**Control**:
- Complete, up-to-date inventory of all public, partner, and internal APIs
- Versioning + deprecation policy
- Documentation as code (OpenAPI + automated publishing)
**Tool**: API Gateway + service catalog (Backstage, etc.)

## General Rule

For every OWASP risk, ask:
1. Can I prevent it at design time? (threat modeling + requirements)
2. Can I detect it in code/pipeline? (SAST, SCA, config scanning)
3. Can I block it at runtime? (WAF, middleware, rate limiting, threat intel)
4. Can I respond and learn from it? (logging + incident process)

The best teams do all four.
