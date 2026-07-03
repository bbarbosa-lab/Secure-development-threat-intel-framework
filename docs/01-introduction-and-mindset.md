# 01 - Introduction and Mindset: Why "Threat-Informed" Secure Development?

## The Problem This Framework Solves

Most development teams still treat security as a **phase** or a **checklist** that only appears at the end of the sprint or after an auditor finds something.

This creates two serious problems:

1. **False sense of security** — "We passed SAST/DAST, so we're secure."
2. **Expensive reactivity** — Vulnerabilities are discovered late, when the cost of fixing them is 10x–100x higher.

The result? Applications that are "secure enough to pass the scan," but that collapse easily against a real attacker using current techniques (not the ones from five years ago).

## The Correct Mindset

> **Security is not about adding controls. It is about removing the conditions that allow an attack to succeed.**
> In the culture of devsecops this is called Shift Left.

A secure system is one that was **designed from the beginning** to frustrate the techniques real attackers use today.

This requires three things:

- **Understand the adversary** (MITRE ATT&CK + real Threat Intelligence)
- **Know the prioritized risks** for the type of application you are building (OWASP Top 10 Web + API)
- **Have a structured process** that turns this knowledge into design, code, and pipeline decisions (NIST SSDF)

## Why "Threat-Informed"?

Threat Intelligence is not just for SOC or Red Team teams.

When properly used in development, it answers critical questions such as:

- Which attack techniques are currently being used against APIs and web applications? (Talos, MISP, OTX)
- Which new and exploitable CVEs affect the libraries we are using? (NVD)
- What behavior patterns (TTPs) are attackers using against applications similar to ours? (MITRE ATT&CK)
- How are real incidents unfolding in organizations within our sector? (FIRST, SANS ISC)

With these answers, you stop guessing what to protect and start **prioritizing based on evidence**.

## Defense in Depth + Threat Intelligence

The classic concept of **Defense in Depth** (multiple layers of protection) remains valid. However, in 2026, these layers must be **informed by up-to-date threat intelligence**.

**Practical example:**

- **Layer 1:** WAF + API Gateway (blocks known attack patterns)
- **Layer 2:** Threat Intel Middleware (blocks malicious IPs/domains reported in recent feeds)
- **Layer 3:** Strict authorization validation in code (mitigates BOLA — OWASP API Security #1)
- **Layer 4:** Structured logging + anomaly detection (enables fast response and feeds intelligence back into the system)

When one layer fails (and eventually one will), the others continue to hold the defense.

## How This Repository Helps You Adopt This Mindset

This repository is not a list of tools. It is a **thinking model** + **practical mappings** that show how to apply this mindset in the daily work of developers building APIs and web/mobile applications.

The documents and mappings were created to answer questions like:

- How does NIST SSDF connect with OWASP and MITRE ATT&CK in practice?
- Where should threat intelligence (MISP, NVD, Talos, etc.) be injected into the development lifecycle?
- What concrete controls (in code, pipeline, and architecture) mitigate the most relevant risks today?

Once you internalize this model, your thinking about security changes permanently — from *"I'm going to run the scanner"* to *"I'm going to design this endpoint so that ATT&CK technique Txxxx cannot succeed here."*
