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
- Start: 2025-11-25
- End:  2025-11-25
- Test environment details (OS, runtime, DB, browsers): Ubuntu 22.04, postgres, Google Chrome 142.0.7444.162

**Assumptions & constraints:**  
- No load or stress testing performed.

---

# 2️⃣ Executive Summary

**Short summary:**  

The security assessment identified several medium and low-risk vulnerabilities, including missing security headers and absence of anti-CSRF tokens, but no high-risk issues were found. Immediate remediation is recommended for the identified weaknesses to improve the overall security posture.

**Overall risk level:** (Low / Medium / High / Critical)

Medium

**Top 5 immediate actions:**  
1. Implement anti-CSRF tokens in all forms to prevent CSRF attacks.
2. Set Content Security Policy (CSP) headers to mitigate XSS and data injection risks.
3. Add X-Frame-Options or frame-ancestors directive to prevent clickjacking.
4. Ensure X-Content-Type-Options header is set to 'nosniff' for all responses.
5. Review and fix error handling to avoid application error disclosures.

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
| F-01 | 🟠 Medium  | Absence of Anti-CSRF Tokens             | Registration form lacks anti-CSRF tokens, exposing to CSRF attacks.                          | ZAP alert on /register form             |
| F-02 | 🟠 Medium  | Content Security Policy Header Not Set  | CSP header missing, increasing risk of XSS and data injection.                               | ZAP alert on / and /register            |
| F-03 | 🟠 Medium  | Missing Anti-clickjacking Header        | X-Frame-Options or frame-ancestors directive not set, allowing clickjacking.                 | ZAP alert on / and /register            |
| F-04 | 🟡 Low     | Application Error Disclosure            | Error messages may disclose sensitive information (e.g., stack traces, internal errors).      | ZAP alert: HTTP/1.1 500 Internal Error  |
| F-05 | 🟡 Low     | X-Content-Type-Options Header Missing   | 'nosniff' header missing, allowing MIME type confusion attacks.                              | ZAP alert on multiple static resources  |
---

# 5️⃣ OWASP ZAP Test Report (Attachment)

**Purpose:**  
- [OWASP ZAP scan results](./ZAP_Report/zap_report_round1.md).

![alt text](<ZAP_Report/image copy 3.png>)
---
