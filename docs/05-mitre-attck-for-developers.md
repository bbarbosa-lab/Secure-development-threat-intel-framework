# 05 - MITRE ATT&CK for Developers

This document teaches how to use **MITRE ATT&CK** practically in day-to-day software development, especially for those building APIs and web applications.

---

## Why ATT&CK is Different from Other Frameworks

Most security frameworks talk about **vulnerabilities** (CVEs) or **generic risks** (OWASP).

MITRE ATT&CK talks about **how real attackers operate** — the techniques they use to gain initial access, execute code, persist, escalate privileges, and so on.

This completely changes the quality of your threat modeling.

---

## Core Concepts

| Concept     | Definition                                      | Example                          |
|-------------|-------------------------------------------------|----------------------------------|
| **Tactic**  | The attacker's objective                        | Initial Access, Execution, Persistence, Privilege Escalation, Credential Access, Defense Evasion, Discovery, Lateral Movement, Collection, Command and Control, Exfiltration, Impact |
| **Technique** | How the attacker achieves the tactical goal   | T1190 - Exploit Public-Facing Application |
| **Sub-technique** | Specific details of the technique          | T1552.001 - Credentials In Files |

---

## Most Relevant ATT&CK Techniques for API Developers

### 1. Initial Access

| Technique                          | ID    | Description                                      | How it applies to APIs                          |
|------------------------------------|-------|--------------------------------------------------|-------------------------------------------------|
| Exploit Public-Facing Application  | T1190 | Exploiting vulnerabilities in internet-facing applications | Injection, broken access control, SSRF, vulnerable components in public APIs |
| Valid Accounts                     | T1078 | Using valid credentials of legitimate users      | Account takeover, use of leaked credentials, session hijacking |

**Threat modeling question:**
"If I were an attacker, how would I gain initial access to this API without having credentials?"

---

### 2. Execution

| Technique                          | ID    | Description                                      | How it applies to APIs                          |
|------------------------------------|-------|--------------------------------------------------|-------------------------------------------------|
| Command and Scripting Interpreter  | T1059 | Executing commands via interpreters (bash, PowerShell, Python, etc.) | Command injection, template injection, eval of untrusted code |
| Exploit Public-Facing Application  | T1190 | (also execution)                                 | RCE via vulnerabilities in frameworks or dependencies |

**Threat modeling question:**
"Which inputs in my API could allow an attacker to execute code on the server?"

---

### 3. Persistence

| Technique             | ID       | Description                                      | How it applies to APIs                          |
|-----------------------|----------|--------------------------------------------------|-------------------------------------------------|
| Account Manipulation  | T1098    | Modifying accounts to maintain access            | Creating backdoor accounts, modifying privileges via API |
| Web Shell             | T1505.003| Deploying a web shell for persistent access      | Uploading malicious files that allow command execution |

---

### 4. Privilege Escalation

| Technique                              | ID    | Description                                      | How it applies to APIs                          |
|----------------------------------------|-------|--------------------------------------------------|-------------------------------------------------|
| Exploitation for Privilege Escalation  | T1068 | Exploiting vulnerabilities to escalate privileges | Vulnerabilities in components running with elevated privileges |
| Valid Accounts                         | T1078 | Using accounts with more privileges than expected | Broken access control allowing privilege escalation |

**Threat modeling question:**
"If an attacker gains access with low privileges, how could they escalate to higher privileges?"

---

### 5. Credential Access

| Technique                        | ID    | Description                                      | How it applies to APIs                          |
|----------------------------------|-------|--------------------------------------------------|-------------------------------------------------|
| Unsecured Credentials            | T1552 | Finding credentials stored insecurely            | Credentials in code, config files, exposed environment variables, logs |
| Credentials from Password Stores | T1555 | Accessing password managers or caches            | Stealing session tokens, cookies, poorly protected JWTs |

**Threat modeling question:**
"Where does my application store or handle credentials and tokens? How could an attacker access them?"

---

### 6. Defense Evasion

| Technique          | ID    | Description                                      | How it applies to APIs                          |
|--------------------|-------|--------------------------------------------------|-------------------------------------------------|
| Masquerading       | T1036 | Disguising malware or malicious activity         | Fake user-agents, residential proxy IPs, behavior mimicking legitimate users |
| Impair Defenses    | T1562 | Disabling or weakening defense mechanisms        | Disabling logging, modifying security configurations via a compromised API |

---

### 7. Discovery

| Technique                       | ID    | Description                                      | How it applies to APIs                          |
|---------------------------------|-------|--------------------------------------------------|-------------------------------------------------|
| Account Discovery               | T1087 | Discovering information about accounts           | User enumeration via API (user enumeration) |
| Network Service Discovery       | T1046 | Discovering services on the network              | SSRF to map internal infrastructure |
| Permission Groups Discovery     | T1069 | Discovering groups and permissions               | Broken access control allowing listing of roles/permissions |

---

### 8. Collection + Exfiltration

| Technique                        | ID    | Description                                      | How it applies to APIs                          |
|----------------------------------|-------|--------------------------------------------------|-------------------------------------------------|
| Data from Local System           | T1005 | Collecting data from the compromised system      | APIs that excessively expose sensitive data |
| Exfiltration Over C2 Channel     | T1041 | Exfiltrating data through the command and control channel | APIs allowing mass data downloads without proper rate limiting |

---

## How to Perform Threat Modeling with ATT&CK (Step by Step)

### Step 1: Define the Scope
- What functionality / endpoint / flow are you analyzing?
- What type of application is it? (Public API, internal, handling sensitive data, etc.)

### Step 2: Select Relevant Techniques
Use the ATT&CK Navigator or search the MITRE website for techniques commonly used against your type of application.

**Most common techniques against web APIs:**
- T1190 - Exploit Public-Facing Application
- T1078 - Valid Accounts
- T1552 - Unsecured Credentials
- T1134 - Access Token Manipulation
- T1059 - Command and Scripting Interpreter
- T1005 - Data from Local System

### Step 3: For each technique, answer 3 questions

1. **How could an attacker apply this technique against my application?**
   (Be specific: which endpoint, which input, which flow)

2. **What security controls do I already have that mitigate this technique?**
   (Be honest. If there is no control, note it as a gap)

3. **What do I need to implement to mitigate or detect this technique?**
   (Preventive control, detective control, or both)

### Step 4: Document and Prioritize
- Document the identified risks
- Prioritize based on: ease of exploitation + impact + existence of threat intelligence indicating active exploitation

### Step 5: Implement Controls and Tests
- Implement the identified controls
- Create tests (manual or automated) that validate whether the technique has been mitigated

---

## Practical Example: Threat Modeling a User API

**Functionality:** Endpoint `GET /api/v1/users/{user_id}` that returns user data.

**ATT&CK Technique:** T1078 - Valid Accounts + T1134 - Access Token Manipulation

**Analysis:**

| Question | Answer |
|----------|--------|
| How could an attacker apply this technique? | An attacker with a valid JWT token from a regular user tries to access another user's data by changing the `user_id` in the URL or manipulating the token claims |
| Existing controls | Authentication via JWT is implemented. There is no validation that the `user_id` belongs to the authenticated user |
| Identified gap | Broken Access Control (A01) |
| Control to implement | Server-side validation: the `user_id` in the URL must match the `sub` of the JWT or the user must have explicit permission to access other users |
| Recommended test | Automated test attempting to access another user's `user_id` with a regular user's token |

---

## Recommended Tools

| Tool                    | Use                                      | Link |
|-------------------------|------------------------------------------|------|
| **ATT&CK Navigator**    | Visualize and filter techniques by platform, group, etc. | https://mitre-attack.github.io/attack-navigator/ |
| **ATT&CK Website**      | Search for techniques and sub-techniques | https://attack.mitre.org/ |
| **ATT&CK Groups**       | See techniques used by specific threat actor groups | https://attack.mitre.org/groups/ |
| **mitreattack-python**  | Python library to query ATT&CK programmatically | Available on PyPI |

---

## Summary: How to Use ATT&CK in Your Daily Work

| Moment              | How to Use ATT&CK                                      | Deliverable |
|---------------------|--------------------------------------------------------|-------------|
| **Threat Modeling** | Select relevant techniques and answer the 3 questions  | Threat model document per functionality |
| **Code Review**     | Ask "Does this implementation mitigate the identified ATT&CK techniques?" | Security-focused code review |
| **Security Testing**| Create tests that validate mitigation of the techniques | More targeted security tests |
| **Incident Response**| Categorize incidents using ATT&CK tactics and techniques | More structured incident analysis |
| **Team Training**   | Use real ATT&CK examples to teach how attackers think  | Team more security-conscious |

---

## How ATT&CK Helps Prioritize OWASP Top 10 Risks

One of the biggest advantages of using ATT&CK together with OWASP is **prioritization based on real attacker behavior**, not just generic checklists.

| OWASP Risk (2025)               | Most Common ATT&CK Techniques     | Priority Level with Active Threat Intel | Recommendation |
|--------------------------------|-----------------------------------|-----------------------------------------|----------------|
| A01 - Broken Access Control    | T1078, T1134, T1552               | ★★★★★                                   | Implement server-side authorization validation on all endpoints |
| A03 - Injection                | T1190, T1059                      | ★★★★★                                   | Parameterized queries + WAF + rigorous input validation |
| A05 - Security Misconfiguration| T1059, T1562                      | ★★★★☆                                   | CIS Benchmarks + automated hardening + removal of sensitive information in error responses |
| A06 - Vulnerable Components    | T1195                             | ★★★★☆                                   | SCA + prioritization by EPSS + threat intel on active exploitation |
| A07 - Authentication Failures  | T1078, T1110, T1552               | ★★★★☆                                   | Mature Identity Providers + re-authentication on sensitive operations |
| A08 - Software Integrity       | T1195, T1608                      | ★★★★☆                                   | Code signing + SBOM + provenance in the pipeline |

**Practical rule:** When threat intelligence (MISP, Talos, NVD) indicates that a specific technique is being actively exploited against your type of application, elevate the priority of that OWASP risk.

---

## Connection to the Integrated Model (SSDF)

ATT&CK should be used primarily in the **Prepare** and **Design** phases of NIST SSDF:

- **Prepare**: Define which ATT&CK techniques are most relevant to your context (public API, mobile, microservices, etc.).
- **Design**: Perform threat modeling per functionality using the relevant techniques.
- **Produce**: Verify that the code mitigates the identified techniques.
- **Respond**: Categorize incidents using ATT&CK tactics and techniques to continuously improve the threat model.

---

## Practical Lessons from the Course (Daniel Lemeszenski)

From the material on **API Threats** and **BOLA**:

- BOLA (Broken Object Level Authorization) is frequently exploited via **T1078 (Valid Accounts)** + manipulation of object identifiers.
- The most effective prevention is **server-side authorization validation** combined with non-sequential identifiers (UUID/GUID).
- "Less is More" reduces the attack surface and makes Discovery and Collection techniques more difficult.

---

**ATT&CK transforms threat modeling from a generic exercise into a concrete analysis based on how attackers actually operate.**

