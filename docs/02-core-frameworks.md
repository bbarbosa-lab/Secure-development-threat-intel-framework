# 02 - Main Frameworks of Threat-Informed Secure Development

This document briefly explains each framework used in this repository and why it was chosen.

## NIST SSDF v1.1

The most complete framework for structuring the **secure development process** in an organization. Used as the backbone of this model.

## OWASP Top 10 2025 + OWASP Top 10 API 2023

The most widely used lists in the world for prioritizing risks in web applications and APIs. Essential for anyone developing software exposed to the internet.

## MITRE ATT&CK

Allows you to think like a real attacker. Instead of "I'm going to protect against SQL Injection," you think: "How do I mitigate techniques T1059 and T1190 that are being actively used against applications like mine?"

## MITRE CVE + CWE + NVD

Provides objective data on known vulnerabilities. Enables automation of prioritization via CVSS and mapping to root weaknesses (CWE) and OWASP risks.

## STIX / TAXII + MISP

Open standards for exchanging threat intelligence in a structured and automated way. Allows your code to "speak the same language" as professional threat intelligence tools.

## CIS Benchmarks

Prescriptive secure configuration guides for operating systems, containers, databases, and cloud environments. Excellent for the Protect phase of SSDF.

When used together, these frameworks create a much more robust system than using any of them in isolation.
