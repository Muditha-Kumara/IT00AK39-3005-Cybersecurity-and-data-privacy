# Phase 3 Authorization & Endpoint Testing Report

## 1. Introduction

This report documents the results of authorization and endpoint testing for the booking system. Testing was performed using browser-based manual testing, OWASP ZAP, and endpoint discovery tools (Gobuster/wfuzz). The focus is on verifying role-based access control, discovering hidden endpoints, and comparing actual behavior to the official specifications.

---

## 2. Role-Based Authorization Matrix


### 🧑‍🦲 Guest

#### ✅ Can do

```yaml
- action: "View public resource list"
	url: "/"
	observation: "Resource list is visible, no reserver identity shown"
	matches_spec: true
- action: "Access login form"
	url: "/login"
	observation: "Login form is accessible"
	matches_spec: true
- action: "View registered reservations (without identity)"
	url: "/"
	observation: "Reservations are visible, reserver identity hidden"
	matches_spec: true
- action: "Access registration form"
   url: "/register"
   observation: "Registration form is accessible, allows new user registration as reserver and Administrator only over 15 years"
   matches_spec: true
```

#### ❌ Cannot do

```yaml
- action: "Access /reservation"
	url: "/reservation"
	observation: "Redirected to http://localhost:8003/status.html?status=failed&message=%3Cstrong%3EUnauthorized%3C%2Fstrong%3E"
	matches_spec: true
- action: "POST new reservation"
	url: "/api/reservations"
	observation: "Blocked, receives "Status: 404 Not Found""
	matches_spec: true
- action: "Access admin pages"
	url: "/admin/*"
	observation: "Redirected to http://localhost:8003/status.html?status=failed&message=%3Cstrong%3ENot%20Found%3C%2Fstrong%3E"
	matches_spec: true
- action: "Access reserver profile"
	url: "/profile"
	observation: "Redirected to http://localhost:8003/status.html?status=failed&message=%3Cstrong%3ENot%20Found%3C%2Fstrong%3E"
	matches_spec: true
```

![alt text](image-4.png)

![alt text](image-6.png)

---


### 🧑‍💼 Reserver

#### ✅ Can do

```yaml
- action: "POST log to Account"
	url: "/login"
	observation: "Can log to account "Status Code 302 Found""
	matches_spec: true
- action: "GET logout from Account"
	url: "/logout"
	observation: "Can log out from account "Status Code 302 Found""
	matches_spec: true
- action: "POST Reservation"
	url: "/reservation"
	observation: "Can post reservation. "Status Code 302 Found". But same resource can reserve in same time."
	matches_spec: fail
- action: "View own profile"
	url: "/profile"
	observation: "Can not Profile page accessible. "http://localhost:8003/status.html?status=failed&message=%3Cstrong%3ENot%20Found%3C%2Fstrong%3E""
	matches_spec: fail
- action: "Manage resources"
	url: "/resources"
	observation: "Can add resource(But name is not unique)"
	matches_spec: true
```

#### ❌ Cannot do

```yaml
- action: "Access admin user list"
	url: "/admin/users"
	observation: "Blocked, http://localhost:8003/status.html?status=failed&message=%3Cstrong%3ENot%20Found%3C%2Fstrong%3E"
	matches_spec: true
- action: "Delete other users"
	url: "/admin/users/:id"
	observation: "Blocked, http://localhost:8003/status.html?status=failed&message=%3Cstrong%3ENot%20Found%3C%2Fstrong%3E"
	matches_spec: true
- action: "Modify resources"
	url: "/admin/resources"
	observation: "Blocked"
	matches_spec: true
- action: "Escalate privileges via hidden form fields"
	url: "N/A"
	observation: "No privilege escalation possible"
	matches_spec: true
```

![alt text](image-7.png)

![alt text](image-8.png)

---


### 🧑‍💼🛡️ Administrator

#### ✅ Can do

```yaml
- action: "POST log to Account"
	url: "/login"
	observation: "Can log to account "Status Code 302 Found""
	matches_spec: true
- action: "GET logout from Account"
	url: "/logout"
	observation: "Can log out from account "Status Code 302 Found""
	matches_spec: true
- action: "Add a resource"
	url: "POST /resources"
	observation: "Resource creation works"
	matches_spec: true
- action: "Delete a reserver"
	url: "POST /resources"
	observation: "only some reserver can be deleted"
	matches_spec: fail
- action: "Add a reservation"
	url: "POST /reservation"
	observation: "Msg: "new row for relation "booking_reservations" violates check constraint "booking_reservations_check""
	matches_spec: fail
- action: "Manage all reservations"
	url: "POST /resources"
	observation: "Can modify resource including description"
	matches_spec: true

```

#### ❌ Cannot do

```yaml
- action: "View all users"
	url: "/users"
	observation: "Do not load User list. "http://localhost:8003/status.html?status=failed&message=%3Cstrong%3ENot%20Found%3C%2Fstrong%3E"
	matches_spec: fail
```

![alt text](image-10.png)

![alt text](image-11.png)

---


## 3. Hidden/Unreferenced Endpoints


List all hidden or unlinked endpoints discovered (via Gobuster, wfuzz, ffuf, ZAP, etc.), and test their accessibility for each role.

```yaml
- endpoint: "/api/resources"
	discovered_by: "wfuzz"
	guest_access: true
	reserver_access: true
	admin_access: true
	notes: "Public resource list, matches expected behavior."
- endpoint: "/api/reservations"
	discovered_by: "wfuzz"
	guest_access: false
	reserver_access: true
	admin_access: true
	notes: "Reservation API, requires authentication."
- endpoint: "/api/session"
	discovered_by: "wfuzz"
	guest_access: false
	reserver_access: true
	admin_access: true
	notes: "Session endpoint, returns 401 for unauthenticated."
- endpoint: "/api/users"
	discovered_by: "wfuzz"
	guest_access: false
	reserver_access: false
	admin_access: true
	notes: "User management, admin only."
- endpoint: "/api/reservations/1"
	discovered_by: "wfuzz (IDOR test)"
	guest_access: false
	reserver_access: true
	admin_access: true
	notes: "Reservation ID endpoint, accessible if authenticated. Test for IDOR risk."
- endpoint: "/api/reservations/2"
	discovered_by: "wfuzz (IDOR test, after adding another reservation)"
	guest_access: false
	reserver_access: true
	admin_access: true
	notes: "Second reservation ID endpoint, accessible if authenticated. Confirms IDOR test for multiple reservations."
```

![alt text](<image copy.png>)

![alt text](image.png)

After add another reseverion

![alt text](image-1.png)

![alt text](image-2.png)

![alt text](image-12.png)


#### Additional Endpoints Discovered with ffuf and SecLists

```yaml
- endpoint: "/~mail", "/~root", "/~www", "/~webmaster", "/~test", "/~guest", etc.
  discovered_by: "ffuf (SecLists)"
  status: 302
  notes: "All return 302 redirects with no content. Likely default server responses, not real app features."
- endpoint: "/sse", "/mcp", "/mcp/transport", "/mcp/message"
  discovered_by: "ffuf (SecLists)"
  status: 302
  notes: "Return 302 redirects. No evidence of real functionality in the application."
- endpoint: "/dns-query?..."
  discovered_by: "ffuf (SecLists)"
  status: 302
  notes: "302 redirect, likely not implemented by the application."
```

![alt text](image-3.png)

---

## 4. Discrepancies & Security Observations

Summarize any mismatches between the specification and actual implementation, including:

- Over-permissive endpoints
- Missing backend checks
- IDOR or privilege escalation risks
- UI vs API inconsistencies

---

## 5. Summary & Recommendations

Provide a concise summary of your findings and any recommendations for remediation or further testing.

---

## 6. Appendix


## ZAP Automated Security Scan Summary

The application was scanned using OWASP ZAP. The results are summarized below:

| Risk Level      | Number of Alerts |
| -------------- | --------------- |
| High           | 0               |
| Medium         | 0               |
| Low            | 0               |
| Informational  | 3               |

**Informational Alerts:**

- Authentication Request Identified (1 instance)
	Indicates detection of authentication requests; not a vulnerability.
- Session Management Response Identified (3 instances)
	Indicates session tokens in responses; informational only.
- User Agent Fuzzer (96 instances)
	Indicates responses to various user agents; no issues found.

No High, Medium, or Low risk vulnerabilities were detected. Informational alerts do not indicate security flaws but are noted for awareness.

For full details, see: [`zap_report_round4.md`](../BookingSystem-Phase1/ZAP_Report/zap_report_round4.md)
