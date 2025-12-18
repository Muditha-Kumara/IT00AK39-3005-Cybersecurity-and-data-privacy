# 🔐 **Authorization Testing Assignment**

## 🎯 **Goal of the Assignment**

You will investigate **what each role (Guest, Reserver, Administrator)**:

* **CAN do**
* **CANNOT do**

…based on the **official application specifications** and the **actual behavior** of the current Phase 3 implementation.

You will test **all accessible pages, functions, and API endpoints** and then produce:

---

**✔️ A clear and simple test report**

Focused entirely on the three roles and their permissions.

---

**✔️ A ZAP scan report**

Saved as a markdown file named:
**`zap_report_round4.md`**

---

## **🧩 Background: What You Are Testing**

Use the system according to the actual implementation and compare it to the specs provided in the project description (points 1–8 in the scenario).

---

> [!NOTE]
> As a reminder

**You are a novice penetration tester at a company. Your company should implement the following application (specs):**

1. The system is accessed via a web browser.  
2. Users can register and, after registration, log in to the system.  
3. A registered and logged-in user acts as either a resource reserver or an administrator.  
4. The administrator can add, remove, and modify resources and reservations.  
5. The administrator can delete the reserver.  
6. A reserver can book a resource if they are over 15 years old.  
7. Resources can be booked on an hourly basis.  
8. The booking system displays booked resources without requiring login, but does not show the reserver's identity
9. The client, your company, requires that the system complies with GDPR regulations.  
10. The system provider has stated that the software is developed following the Privacy by Design (PbD) principle. 

---

**Imagine that your client wants confirmation that:**

* Guests cannot access protected content
* Reservers cannot perform admin actions
* Administrators have full control but no unnecessary extra exposure
* No endpoint leaks, hidden pages, or bypasses exist
* Authorization decisions are correctly enforced at the backend

---

## 🧭 **Deliverables**

You must return **Two items → In the github repo**

---

## 1️⃣ **🏗️ Create the Main Testing List (your own markdown file)**

You must create **one clear markdown document** where all testing results are organized under **three role-based sections**:

---

### 🧑‍🦲 **Guest**

---

**✅ Can do**

List every action a *Guest* can perform, with the page or endpoint.
Example format:

* “Can view public resource list — `/`”
* “Can access login form — `/login`”
* “Can view registered reservations without identity — `/` (spec 8)”

---

**❌ Cannot do**

List every action that a *Guest* is blocked from doing.
Example format:

* “Cannot access `/reservation` (redirect to login)”
* “Cannot POST `/api/reservations`”
* “Cannot access any `/admin/*` pages”
* “Cannot access reserver profile page `/profile`”

---

### 🧑‍💼 **Reserver**

---

**✅ Can do**

List actions a *Reserver* can do according to specs + actual test results.
Include visible pages **and** API endpoints.

Example format:

* “Can book a resource — `/reservation` + `/api/reservations`”
* “Can view own profile page — `/profile`”
* “Can list resources — `/resources`”

---

**❌ Cannot do**

List actions a *Reserver* is correctly blocked from.

Example format:

* “Cannot access admin user list — `/admin/users`”
* “Cannot delete other users — `/api/admin/users/:id`”
* “Cannot modify resources (spec says admin only)”
* “Cannot escalate privileges via hidden form fields”

---

### 🧑‍💼🛡️ **Administrator**

---

**✅ Can do**

List actions an *Administrator* can perform.

Example format:

* “Can add a resource — `/admin/resources/new`”
* “Can delete a reserver — `/admin/users/delete/:id`”
* “Can manage all reservations — `/admin/reservations`”
* “Can view all users (spec 4)”

---

**❌ Cannot do**

List prohibited behaviors, if any, or incorrect implementation issues.

Example format:

* “Cannot book a resource if the system incorrectly blocks admins (bug?)”
* “Cannot perform an action because the UI has no link (but API allows?) — flag as ⚠️”

---

### 🔍 **Important Notes**

**✔️ Each bullet point must include (Remember to be brief → Brings clarity):**

* **Action**
* **Where it happens** (URL, endpoint, or form)
* **Observations**
* **Whether behavior matches the spec**

---

**✔️ Each point must be filled during (iterative approach):**

1. **Browser testing**
2. **ZAP testing**
3. **Gobuster/wfuzz endpoint discovery**

---

> [!NOTE]
> ✔️ Hidden pages found with Gobuster or ZAP must also be added under the correct role.

---

## 2️⃣ **📦 ZAP Scan Report (required format)**

Name the file **exactly**:

```
zap_report_round4.md
```

## 3️⃣ **☝️ link to the github repo where the previous two documents can be found**

---


## 🌐 **Testing Phase 1 →  Browser Testing**

Start with the browser as a normal end user.

**Tasks:**

1. Create test accounts:

   * Guest = not logged in (no need to create account)
   * Reserver
   * Administrator

2. Perform every visible action:

   * View pages
   * Submit forms
   * Change URLs manually
   * Attempt to access admin pages as a Reserver
   * Attempt to access reserver-only pages as a Guest
   * ...

3. For every page or function you test:

   * Update the list
   * Record discrepancies between specs and implementation

---

**Key idea:**

Try breaking the rules.

---

## 🧪 **Testing Phase 2 → ZAP Testing**

Use OWASP ZAP to:

* Discover hidden pages
* Examine role-based access
* Detect authorization issues like IDOR, missing access checks
* Explore API behavior

**Tasks:**

1. Run a scan.
2. Explore the site in authenticated mode for both roles.
3. Compare ZAP findings to your list:

   * Did ZAP find pages you did not?
   * Did ZAP detect insecure authorization behavior?
4. Export the results as **markdown**
5. Name the file **`zap_report_round4.md`**

Add any new findings to your main list.

---

## 🧭 **Testing Phase 3 → Gobuster / wfuzz / ffuf**

Now test for hidden or unreferenced endpoints.

**Examples:**

**General directory discovery:**

```bash
gobuster dir -u http://localhost:8000 -w /usr/share/wordlists/dirb/common.txt
```

or:

```bash
wfuzz -c -w /usr/share/wordlists/dirb/common.txt --hc 404 http://localhost:8000/FUZZ
```

**API endpoint discovery:**

```bash
wfuzz -c -w /usr/share/wordlists/dirb/common.txt --hc 404 http://localhost:8000/api/FUZZ
```

**Search for reservation IDs:**

```bash
wfuzz -c -z range,1-1000 --hc 404 http://localhost:8000/api/reservations/FUZZ
```

---

**Your job:**

* Add all discovered endpoints to your list
* Test access for Guest / Reserver / Admin
* Note any unexpected behavior
* Verify backend authorization (not just frontend UI)

---

**Critical finding examples to watch for:**

* Guest can access `/api/resources` ❌
* Reserver can delete other users ❌
* Reserver can access `/admin` without UI links ⚠️
* ID-based pages reveal other users' bookings ❌

---

## ✍️ **Final Consolidation**

At this point, you should:

✔️ Have a complete list of all pages and endpoints  
✔️ Have accurate role-based permissions in the list  
✔️ Have ZAP's findings integrated  
✔️ Have hidden pages tested via Gobuster/wfuzz  
✔️ Have evaluated the implementation against specs 

---

**Now:**

1. Re-walk the app with your final list.
2. Correct mistakes or update findings.

---

## 🧾 Final Output to Return (Github repo link)

### 📌 **File 1 → Your Authorization Test Report**

Markdown file with:

* Completed list
* Findings
* Summary of role capabilities

Name:

```
auth_test_report.md
```

---

### 📌 **File 2 → ZAP Report**

Markdown file named:

```
zap_report_round4.md
```

---

## 🎉 You're Done!

This assignment replicates the workflow of a real junior penetration tester, focusing on:

* Role analysis
* Discovery of hidden endpoints
* Backend vs frontend authorization
* Validation of business requirements
* Tool-assisted access control testing