---
# Reporting Findings and Fixes

This post is the official report to the development team, summarizing the Top 5 immediate actions identified in Part 1 (see `TestReport.md`) and their current fix status. Each finding is addressed individually, with a description of how it was originally identified, how the fix status was verified using the ZAP reports from 2025-11-25 and 2025-12-02, and clear documentation of the verification steps. Please see the attached screenshots for evidence.

---

## Top 5 Immediate Actions: Review and Fix Status

### 1. Absence of Anti-CSRF Tokens

- **Original Identification (Part 1):**  
	Reported in `TestReport.md` as F-01. ZAP flagged the registration form (`/register`) for lacking anti-CSRF tokens, exposing the application to CSRF attacks.
- **How Fix Was Verified:**  
	Reviewed both ZAP reports:
	- **2025-11-25:** Alert present for `/register` (Medium risk).
	- **2025-12-02:** Alert still present for `/register` (Medium risk).
- **Verification Steps:**
	1. Opened `2025-12-02-ZAP-Report-.md` and searched for "Absence of Anti-CSRF Tokens".
	2. Confirmed the alert is still present for the registration form.
	3. Compared with the previous report to confirm no change.
- **Status:**  
	**Not Fixed**
- **Evidence:**  
	![Screenshot: ZAP Report Dec 2 - Absence of Anti-CSRF Tokens](#)

---

### 2. Content Security Policy (CSP) Header Not Set

- **Original Identification (Part 1):**  
	Reported in `TestReport.md` as F-02. ZAP flagged missing CSP headers on `/` and `/register`, increasing XSS and data injection risk.
- **How Fix Was Verified:**  
	- **2025-11-25:** Alert present for both endpoints (Medium risk).
	- **2025-12-02:** No alert for CSP header missing.
- **Verification Steps:**
	1. Opened `2025-12-02-ZAP-Report-.md` and checked the Alerts section.
	2. Confirmed the absence of any CSP header alert.
	3. Compared with the previous report to confirm the alert was present before.
- **Status:**  
	**Fixed**
- **Evidence:**  
	![Screenshot: ZAP Report Nov 25 - CSP Header Not Set](#)
	![Screenshot: ZAP Report Dec 2 - No CSP Header Alert](#)

---

### 3. Missing Anti-clickjacking Header

- **Original Identification (Part 1):**  
	Reported in `TestReport.md` as F-03. ZAP flagged missing X-Frame-Options or frame-ancestors directive on `/` and `/register`.
- **How Fix Was Verified:**  
	- **2025-11-25:** Alert present for both endpoints (Medium risk).
	- **2025-12-02:** No alert for missing anti-clickjacking header.
- **Verification Steps:**
	1. Opened `2025-12-02-ZAP-Report-.md` and checked the Alerts section.
	2. Confirmed the absence of any anti-clickjacking header alert.
	3. Compared with the previous report to confirm the alert was present before.
- **Status:**  
	**Fixed**
- **Evidence:**  
	![Screenshot: ZAP Report Nov 25 - Missing Anti-clickjacking Header](#)
	![Screenshot: ZAP Report Dec 2 - No Anti-clickjacking Alert](#)

---

### 4. Application Error Disclosure

- **Original Identification (Part 1):**  
	Reported in `TestReport.md` as F-04. ZAP detected error messages (e.g., HTTP 500 Internal Server Error) that could disclose sensitive information.
- **How Fix Was Verified:**  
	- **2025-11-25:** Alert present (Low risk).
	- **2025-12-02:** No alert for application error disclosure.
- **Verification Steps:**
	1. Opened `2025-12-02-ZAP-Report-.md` and checked the Alerts section.
	2. Confirmed the absence of any application error disclosure alert.
	3. Compared with the previous report to confirm the alert was present before.
- **Status:**  
	**Fixed**
- **Evidence:**  
	![Screenshot: ZAP Report Nov 25 - Application Error Disclosure](#)
	![Screenshot: ZAP Report Dec 2 - No Application Error Disclosure](#)

---

### 5. X-Content-Type-Options Header Missing

- **Original Identification (Part 1):**  
	Reported in `TestReport.md` as F-05. ZAP flagged missing 'nosniff' header on multiple static resources.
- **How Fix Was Verified:**  
	- **2025-11-25:** Alert present (Low risk).
	- **2025-12-02:** No alert for missing X-Content-Type-Options header.
- **Verification Steps:**
	1. Opened `2025-12-02-ZAP-Report-.md` and checked the Alerts section.
	2. Confirmed the absence of any X-Content-Type-Options header alert.
	3. Compared with the previous report to confirm the alert was present before.
- **Status:**  
	**Fixed**
- **Evidence:**  
	![Screenshot: ZAP Report Nov 25 - X-Content-Type-Options Header Missing](#)
	![Screenshot: ZAP Report Dec 2 - No X-Content-Type-Options Alert](#)

---

## Summary Table

| Finding                                 | Status     |
|------------------------------------------|------------|
| Absence of Anti-CSRF Tokens              | Not Fixed  |
| Content Security Policy (CSP) Header     | Fixed      |
| Missing Anti-clickjacking Header         | Fixed      |
| Application Error Disclosure             | Fixed      |
| X-Content-Type-Options Header Missing    | Fixed      |

---

**Note:** Screenshots should be attached for each verification step as evidence.
---

### 1. Absence of Anti-CSRF Tokens

- **Original Identification:**  
	In Part 1, this issue was identified using ZAP by Checkmarx, which flagged the lack of anti-CSRF tokens in HTML forms (see `/register` endpoint).
- **Verification Process:**  
	I reviewed the latest ZAP reports (`2025-11-25-ZAP-Report.md` and `2025-12-02-ZAP-Report-.md`). The issue persists in both, with the most recent scan (Dec 2, 2025) still reporting the absence of anti-CSRF tokens.
- **Verification Steps:**  
	1. Opened the ZAP report for Dec 2, 2025.
	2. Located the "Absence of Anti-CSRF Tokens" alert.
	3. Confirmed the alert is still present for the `/register` form.
- **Status:**  
	**Not Fixed**
- **Evidence:**  
	*(Insert screenshot: ZAP Report showing Absence of Anti-CSRF Tokens)*

---

### 2. Content Security Policy (CSP) Header Not Set

- **Original Identification:**  
	Detected by ZAP in Part 1, which reported missing CSP headers on key endpoints.
- **Verification Process:**  
	Checked both ZAP reports. The Nov 25, 2025 report lists this as a Medium risk, but the Dec 2, 2025 report does not mention it, suggesting it may have been fixed.
- **Verification Steps:**  
	1. Compared the alerts section in both reports.
	2. Noted the absence of the CSP header alert in the latest report.
- **Status:**  
	**Fixed**
- **Evidence:**  
	*(Insert screenshot: ZAP Report Nov 25 showing CSP alert)*  
	*(Insert screenshot: ZAP Report Dec 2 showing no CSP alert)*

---

### 3. Missing Anti-clickjacking Header

- **Original Identification:**  
	ZAP flagged missing anti-clickjacking headers (X-Frame-Options or CSP frame-ancestors) in Part 1.
- **Verification Process:**  
	The Nov 25, 2025 report lists this as a Medium risk. The Dec 2, 2025 report does not mention it, indicating the issue has likely been addressed.
- **Verification Steps:**  
	1. Reviewed both ZAP reports for the presence of this alert.
	2. Confirmed its absence in the latest scan.
- **Status:**  
	**Fixed**
- **Evidence:**  
	*(Insert screenshot: ZAP Report Nov 25 showing Anti-clickjacking alert)*  
	*(Insert screenshot: ZAP Report Dec 2 showing no Anti-clickjacking alert)*

---

### 4. Application Error Disclosure

- **Original Identification:**  
	ZAP detected error messages that could disclose sensitive information (e.g., HTTP 500 Internal Server Error).
- **Verification Process:**  
	The Nov 25, 2025 report lists this as a Low risk. The Dec 2, 2025 report does not mention it, suggesting the issue has been resolved.
- **Verification Steps:**  
	1. Checked the alert details in both reports.
	2. Verified the absence of this alert in the latest scan.
- **Status:**  
	**Fixed**
- **Evidence:**  
	*(Insert screenshot: ZAP Report Nov 25 showing Application Error Disclosure)*  
	*(Insert screenshot: ZAP Report Dec 2 showing no Application Error Disclosure)*

---

### 5. X-Content-Type-Options Header Missing

- **Original Identification:**  
	ZAP reported missing X-Content-Type-Options headers on several endpoints and static files.
- **Verification Process:**  
	The Nov 25, 2025 report lists this as a Low risk. The Dec 2, 2025 report does not mention it, indicating the header has been added.
- **Verification Steps:**  
	1. Reviewed the alerts in both reports.
	2. Confirmed the alert is not present in the Dec 2, 2025 report.
- **Status:**  
	**Fixed**
- **Evidence:**  
	*(Insert screenshot: ZAP Report Nov 25 showing X-Content-Type-Options alert)*  
	*(Insert screenshot: ZAP Report Dec 2 showing no X-Content-Type-Options alert)*

---

## Summary Table

| Finding                                 | Status     |
|------------------------------------------|------------|
| Absence of Anti-CSRF Tokens              | Not Fixed  |
| Content Security Policy (CSP) Header     | Fixed      |
| Missing Anti-clickjacking Header         | Fixed      |
| Application Error Disclosure             | Fixed      |
| X-Content-Type-Options Header Missing    | Fixed      |

---

**Note:** Screenshots should be attached for each verification step as evidence.
