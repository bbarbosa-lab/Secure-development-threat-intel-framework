# FastAPI Threat Shield

**Production-ready middleware example** that brings threat intelligence directly into your API layer.

This is the flagship practical example of the Threat-Informed Secure Development framework.

## What It Does

- Checks incoming requests against multiple threat intelligence sources (AbuseIPDB + local MISP instance)
- Blocks or rate-limits known malicious IPs and domains in real time
- Logs security-relevant events in a structured way (ready to be exported to STIX format)
- Provides a clean pattern you can extend with your own policies and feeds

## Why This Matters

Most "secure" APIs still trust every request that reaches the application code.  
This example shows how to add an **intelligence-informed edge** that makes decisions based on real, current threat data — not just static rules.

It directly addresses:
- OWASP API Security Risks (especially Broken Authentication, BOLA patterns, and runtime protection)
- NIST SSDF PS.3 and RV.2 practices
- Real attacker infrastructure (C2 servers, scanners, known malicious actors)

## Project Structure

```
fastapi-threat-shield/
├── app/
│   ├── main.py                 # FastAPI app with middleware
│   ├── middleware/
│   │   └── threat_intel.py     # The core Threat Intel middleware
│   ├── config.py
│   └── utils/
├── tests/
├── docker-compose.yml          # Includes MISP instance for local testing
├── requirements.txt
└── README.md
```

## Quick Start (Local)

```bash
cd examples/fastapi-threat-shield
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```

Then call the protected endpoint and watch the middleware in action.

## Extending It

- Add your own MISP instance (highly recommended)
- Integrate Talos or other commercial feeds
- Export security events to your SIEM in STIX format
- Add behavioral analysis on top of reputation checks

This pattern scales from startups to large enterprises and demonstrates exactly the kind of thinking that distinguishes good AppSec/DevSecOps professionals.
