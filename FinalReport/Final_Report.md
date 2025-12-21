# Final Report

---

## PortSwigger

**Screenshot of Completed Labs:**

![alt text](image.png)

**Completed Labs List:**

### SQL injection

- SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
- SQL injection vulnerability allowing login bypass
- SQL injection attack, querying the database type and version on Oracle
- SQL injection attack, querying the database type and version on MySQL and Microsoft
- SQL injection attack, listing the database contents on non-Oracle databases
- SQL injection attack, listing the database contents on Oracle
- SQL injection UNION attack, determining the number of columns returned by the query
- SQL injection UNION attack, finding a column containing text
- SQL injection UNION attack, retrieving data from other tables
- SQL injection UNION attack, retrieving multiple values in a single column

### Access control vulnerabilities

- Unprotected admin functionality
- Unprotected admin functionality with unpredictable URL
- User role can be modified in user profile
- User ID controlled by request parameter 

### Authentication

- Username enumeration via different responses
- Password reset broken logic
- Username enumeration via subtly different responses
- Username enumeration via response timing

---

## The Booking System Project

### Phase 1: Initial Security Assessment

**What was done:**
	- Deployed the booking system using Docker Compose.
	- Performed a comprehensive vulnerability assessment using OWASP ZAP.
	- The ZAP scan identified the following issues (see [zap_report_round1.md](https://github.com/Muditha-Kumara/IT00AK39-3005-Cybersecurity-and-data-privacy/blob/main/BookingSystem-Phase1/ZAP_Report/zap_report_round1.md)):
		- **High risk:**
			- Path Traversal (on /register, POST username)
			- SQL Injection (on /register, POST username)
		- **Medium risk:**
			- Absence of Anti-CSRF Tokens (on /register form)
			- Content Security Policy (CSP) Header Not Set (on / and /register)
			- Missing Anti-clickjacking Header (on / and /register)
		- **Low risk:**
			- Application Error Disclosure (HTTP 500 Internal Error on /register)
			- X-Content-Type-Options Header Missing (on multiple static resources)

**What worked:**
	- ZAP scans provided detailed and actionable findings for all major vulnerability categories.
    - Did PortSwigger tests and get idea about SQL injection, Access control vulnerabilities, Authentication etc vulnerabilities.
	- The Dockerized environment ensured consistent and reproducible testing.

**What didn't work:**
	- First I did with Chrome header. Then did not detect SQL injection risk.

**Most time-consuming:**
	- Identify Chrome header issue with ZAP.

**What I learned:**
	- I understand basic idea of security risks and how tests a web application.
    - Automated tools like ZAP are essential for uncovering a wide range of vulnerabilities, from critical injection flaws to missing security headers.
	- Even simple web applications can have multiple layers of security issues that require both technical and configuration fixes.

### Phase 2: Password Cracking
- **What was done:**
	- Used hashcat with `rockyou.txt` and `crackstation.txt` to crack password hashes (see `Phase2/Phase 2: Password Cracking.md`).
	- Compared dictionary and non-dictionary attacks.
- **What worked:**
	- Dictionary attacks were fast and effective for weak passwords.
- **What didn't work:**
	- Brute-force attacks were impractical for longer passwords.
- **Most time-consuming:**
	- Running hashcat on large wordlists.
- **What I learned:**
	- Password length and complexity are critical for security.

### Phase 3: Authorization & Endpoint Testing
- **What was done:**
	- Tested role-based access (Guest, Reserver, Admin) and endpoint security (see `Phase3/auth_test_report.md.md`).
	- Used ZAP and manual testing to verify access controls.
- **What worked:**
	- Role-based restrictions were mostly enforced as per spec.
- **What didn't work:**
	- Some endpoints initially exposed more information than intended.
- **Most time-consuming:**
	- Mapping all endpoints and testing each role.
- **What I learned:**
	- The value of thorough endpoint discovery and access control testing.

### Phase 4: Privacy, GDPR, and Policies
- **What was done:**
	- Drafted and reviewed privacy policy, cookie policy, terms of service, and GDPR checklist (see `Phase4/`).
- **What worked:**
	- Policies were aligned with GDPR requirements.
- **What didn't work:**
	- Ensuring all technical controls matched policy statements required extra review.
- **Most time-consuming:**
	- Mapping data flows and verifying compliance.
- **What I learned:**
	- Legal and technical compliance must go hand-in-hand.

#### Reflection
> This project provided hands-on experience in identifying, exploiting, and remediating common web vulnerabilities. I learned the importance of both technical and policy controls in building secure systems. The iterative process of testing, fixing, and retesting was invaluable for understanding real-world security challenges.

---

## Logbook

- **GitHub Repository:** [https://github.com/Muditha-Kumara/IT00AK39-3005-Cybersecurity-and-data-privacy](https://github.com/Muditha-Kumara/IT00AK39-3005-Cybersecurity-and-data-privacy)

- **Total hours spent:** 52
- **Hours per topic:**
	- PortSwigger Labs: 8
	- Phase 1 (Assessment): 12
	- Phase 2 (Password Cracking): 8
	- Phase 3 (Authorization): 12
	- Phase 4 (Policies & GDPR): 8
	- Reporting & Documentation: 4

---

## Feedback (Optional)

- The course provided a comprehensive overview of both offensive and defensive security.
- More real-world case studies and group discussions would further enhance learning.
- The hands-on labs and project-based approach were especially valuable.

---
