# 05 - MITRE ATT&CK for Developers

This document explains how to use **MITRE ATT&CK** practically in day-to-day software development, especially when building APIs and web applications.

## Why ATT&CK is Different from Other Frameworks

Most security frameworks focus on **vulnerabilities** (CVEs) or **generic risks** (OWASP Top 10).

MITRE ATT&CK focuses on **how real attackers operate** — the specific techniques they use to gain initial access, execute code, persist, escalate privileges, evade defenses, and achieve their objectives.

This shift significantly improves the quality and realism of your threat modeling.

## Core Concepts

| Concept     | Definition                                      | Example                          |
|-------------|-------------------------------------------------|----------------------------------|
| **Tactic**  | The attacker's objective                        | Initial Access, Execution, Persistence, Privilege Escalation, Credential Access |
| **Technique** | How the attacker achieves a tactical objective | T1190 – Exploit Public-Facing Application |
| **Sub-technique** | More specific variant of a technique        | T1552.001 – Credentials In Files |

## Most Relevant ATT&CK Techniques for API Developers

### 1. Initial Access

| Technique                              | ID     | Description                                      | How It Applies to APIs                          |
|----------------------------------------|--------|--------------------------------------------------|-------------------------------------------------|
| Exploit Public-Facing Application      | T1190  | Exploit vulnerabilities in internet-facing apps  | Injection, broken access control, SSRF, vulnerable components |
| Valid Accounts                         | T1078  | Use legitimate user credentials                  | Account takeover, leaked credentials, session hijacking |

**Threat Modeling Question:**  
"If I were an attacker, how would I gain initial access to this API without valid credentials?"

### 2. Execution

| Technique                              | ID     | Description                                      | How It Applies to APIs                          |
|----------------------------------------|--------|--------------------------------------------------|-------------------------------------------------|
| Command and Scripting Interpreter      | T1059  | Execute commands via interpreters (bash, PowerShell, Python, etc.) | Command injection, template injection, unsafe `eval` |
| Exploit Public-Facing Application      | T1190  | Also used for remote code execution              | RCE via framework or dependency vulnerabilities |

**Threat Modeling Question:**  
"Which inputs in my API could allow an attacker to execute code on the server?"

### 3. Persistence

| Technique                  | ID          | Description                              | How It Applies to APIs                     |
|----------------------------|-------------|------------------------------------------|--------------------------------------------|
| Account Manipulation       | T1098       | Modify accounts to maintain access       | Backdoor account creation, privilege changes via API |
| Web Shell                  | T1505.003   | Deploy web shell for persistent access   | Malicious file uploads enabling command execution |

### 4. Privilege Escalation

| Technique                              | ID     | Description                                      | How It Applies to APIs                          |
|----------------------------------------|--------|--------------------------------------------------|-------------------------------------------------|
| Exploitation for Privilege Escalation  | T1068  | Exploit vulnerabilities to gain higher privileges | Vulnerabilities in components running with elevated privileges |
| Valid Accounts                         | T1078  | Use accounts with more privileges than expected  | Broken access control enabling privilege escalation |

**Threat Modeling Question:**  
"If an attacker gains low-privilege access, how could they escalate to higher privileges?"

### 5. Credential Access

| Technique                     | ID     | Description                                      | How It Applies to APIs                          |
|-------------------------------|--------|--------------------------------------------------|-------------------------------------------------|
| Unsecured Credentials         | T1552  | Find credentials stored insecurely               | Credentials in code, config files, environment variables, or logs |
| Credentials from Password Stores | T1555 | Access password managers or caches            | Stealing session tokens, cookies, or poorly protected JWTs |

**Threat Modeling Question:**  
"Where does my application store or handle credentials and tokens? How could an attacker access them?"

### 6. Defense Evasion

| Technique             | ID     | Description                                      | How It Applies to APIs                          |
|-----------------------|--------|--------------------------------------------------|-------------------------------------------------|
| Masquerading          | T1036  | Disguise malicious activity                      | Fake user-agents, residential proxy IPs, behavior mimicking legitimate users |
| Impair Defenses       | T1562  | Disable or weaken defensive mechanisms           | Disabling logging or modifying security settings via compromised API |

### 7. Discovery

| Technique                        | ID     | Description                              | How It Applies to APIs                     |
|----------------------------------|--------|------------------------------------------|--------------------------------------------|
| Account Discovery                | T1087  | Discover information about accounts      | User enumeration via API endpoints         |
| Network Service Discovery        | T1046  | Discover services on the network         | SSRF used to map internal infrastructure   |
| Permission Groups Discovery      | T1069  | Discover groups and permissions          | Broken access control allowing role/permission enumeration |

### 8. Collection + Exfiltration

| Technique                        | ID     | Description                                      | How It Applies to APIs                          |
|----------------------------------|--------|--------------------------------------------------|-------------------------------------------------|
| Data from Local System           | T1005  | Collect data from the compromised system         | APIs that excessively expose sensitive data     |
| Exfiltration Over C2 Channel     | T1041  | Exfiltrate data through command and control      | APIs allowing mass data downloads without proper rate limiting |

## How to Perform Threat Modeling with ATT&CK (Step by Step)

### Step 1: Define the Scope
- What functionality, endpoint, or user flow are you analyzing?
- What type of application is it? (Public API, internal, handling sensitive data, etc.)

### Step 2: Select Relevant Techniques
Use the ATT&CK Navigator or search the MITRE website for techniques commonly used against your type of application.

**Most common techniques against web APIs:**
- T1190 – Exploit Public-Facing Application
- T1078 – Valid Accounts
- T1552 – Unsecured Credentials
- T1134 – Access Token Manipulation
- T1059 – Command and Scripting Interpreter
- T1005 – Data from Local System

### Step 3: For Each Technique, Answer These 3 Questions

1. **How could an attacker apply this technique against my application?**  
   (Be specific: which endpoint, input, or flow)

2. **What security controls do I already have that mitigate this technique?**  
   (Be honest. If none exist, document it as a gap.)

3. **What do I need to implement to mitigate or detect this technique?**  
   (Preventive control, detective control, or both)

### Step 4: Document and Prioritize
- Document the identified risks.
- Prioritize based on: ease of exploitation + potential impact + evidence of active exploitation from threat intelligence.

### Step 5: Implement Controls and Tests
- Implement the identified controls.
- Create tests (manual or automated) that validate whether the technique has been mitigated.

## Practical Example: Threat Modeling a User API Endpoint

**Functionality:** `GET /api/v1/users/{user_id}` endpoint that returns user data.

**ATT&CK Techniques:** T1078 – Valid Accounts + T1134 – Access Token Manipulation

**Analysis:**

| Question                                      | Response |
|-----------------------------------------------|----------|
| How could an attacker apply this technique?   | An attacker with a valid JWT from a regular user tries to access another user's data by changing the `user_id` in the URL or manipulating JWT claims. |
| Existing controls                             | JWT authentication is implemented. There is no validation ensuring the `user_id` belongs to the authenticated user. |
| Gap identified                                | Broken Access Control (A01:2025) |
| Control to implement                          | Server-side validation: the `user_id` in the URL must match the `sub` claim of the JWT, or the user must have explicit permission to access other users. |
| Recommended test                              | Automated test attempting to access another user's `user_id` using a regular user's token. |

## Recommended Tools

| Tool                    | Purpose                                      | Link |
|-------------------------|----------------------------------------------|------|
| **ATT&CK Navigator**    | Visualize and filter techniques by platform, group, etc. | https://mitre-attack.github.io/attack-navigator/ |
| **ATT&CK Website**      | Search techniques and sub-techniques         | https://attack.mitre.org/ |
| **ATT&CK Groups**       | View techniques used by specific threat actors | https://attack.mitre.org/groups/ |
| **mitreattack-python**  | Python library to query ATT&CK programmatically | Available on PyPI |

## Summary: Using ATT&CK in Your Daily Work

| Activity            | How to Use ATT&CK                                      | Deliverable                              |
|---------------------|--------------------------------------------------------|------------------------------------------|
| **Threat Modeling** | Select relevant techniques and answer the 3 questions  | Threat model document per functionality  |
| **Code Review**     | Ask: "Does this implementation mitigate the identified ATT&CK techniques?" | Security-focused code reviews            |
| **Security Testing**| Create tests that validate mitigation of techniques    | More targeted security tests             |
| **Incident Response**| Categorize incidents using ATT&CK tactics and techniques | More structured incident analysis        |
| **Team Training**   | Use real ATT&CK examples to teach how attackers think  | Team with better security awareness      |

---

**ATT&CK transforms threat modeling from a generic exercise into a concrete analysis based on how real attackers operate.**

**Next:** Section 06 will cover structured threat intelligence (STIX, TAXII, MISP) and how to integrate it into your code and processes.
