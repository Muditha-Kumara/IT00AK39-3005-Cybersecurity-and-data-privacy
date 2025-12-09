# 1️⃣ Introduction

**Tester(s):**  
- Name: Muditha.Kumara

**Purpose:**  
- identify vulnerabilities in registration and authentication flows.

**Scope:**  
- Tested components: main landing page (/), registration page (/register)
- Exclusions: None
- Test approach: Gray-box

**Test environment & dates:**  
- Start: 2025-12-09
- End:  2025-12-09
- Test environment details (OS, runtime, DB, browsers): Ubuntu 22.04, postgres, Google Chrome Headless 142.0.7444.162

**Assumptions & constraints:**  
- No load or stress testing performed.

---

# 2️⃣ Executive Summary


**Short summary:**  

The security assessment identified two high-risk vulnerabilities (Path Traversal and SQL Injection), as well as several medium and low-risk issues, including missing security headers and absence of anti-CSRF tokens. Immediate remediation is required for the high-risk findings, and recommended for the remaining weaknesses to improve the overall security posture.

**Overall risk level:** (Low / Medium / High / Critical)

High

**Top 5 immediate actions:**  
1. Fix Path Traversal vulnerability by validating and sanitizing all user-supplied file paths.
2. Fix SQL Injection by using parameterized queries and input validation.
3. Implement anti-CSRF tokens in all forms to prevent CSRF attacks.
4. Set Content Security Policy (CSP) headers to mitigate XSS and data injection risks.
5. Add X-Frame-Options or frame-ancestors directive to prevent clickjacking.

---

# 3️⃣ Severity scale & definitions

|  **Severity Level**  | **Description**                                                                                                              | **Recommended Action**           |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
|      🔴 **High**     | A serious vulnerability that can lead to full system compromise or data breach (e.g., SQL Injection, Remote Code Execution). | *Immediate fix required*         |
|     🟠 **Medium**    | A significant issue that may require specific conditions or user interaction (e.g., XSS, CSRF).                              | *Fix ASAP*                       |
|      🟡 **Low**      | A minor issue or configuration weakness (e.g., server version disclosure).                                                   | *Fix soon*                       |
| 🔵 **Info** | No direct risk, but useful for system hardening (e.g., missing security headers).                                            | *Monitor and fix in maintenance* |


---

# 4️⃣ Findings

| ID   | Severity   | Finding                                 | Description                                                                                  | Evidence / Proof                        |
|------|------------|-----------------------------------------|----------------------------------------------------------------------------------------------|-----------------------------------------|
| F-01 | 🔴 High    | Path Traversal                          | User input not properly validated, allowing access to files outside intended directories.     | ZAP alert on /register (POST username)  |
| F-02 | 🔴 High    | SQL Injection                           | User input not properly sanitized, allowing SQL injection in registration.                   | ZAP alert on /register (POST username)  |
| F-03 | 🟠 Medium  | Absence of Anti-CSRF Tokens             | Registration form lacks anti-CSRF tokens, exposing to CSRF attacks.                          | ZAP alert on /register form             |
| F-04 | 🟠 Medium  | Content Security Policy Header Not Set  | CSP header missing, increasing risk of XSS and data injection.                               | ZAP alert on / and /register            |
| F-05 | 🟠 Medium  | Missing Anti-clickjacking Header        | X-Frame-Options or frame-ancestors directive not set, allowing clickjacking.                 | ZAP alert on / and /register            |
| F-06 | 🟡 Low     | Application Error Disclosure            | Error messages may disclose sensitive information (e.g., stack traces, internal errors).      | ZAP alert: HTTP/1.1 500 Internal Error  |
| F-07 | 🟡 Low     | X-Content-Type-Options Header Missing   | 'nosniff' header missing, allowing MIME type confusion attacks.                              | ZAP alert on multiple static resources  |
---

# 5️⃣ OWASP ZAP Test Report (Attachment)

**Purpose:**  
- [OWASP ZAP scan results](./ZAP_Report/zap_report_round1.md).

---
