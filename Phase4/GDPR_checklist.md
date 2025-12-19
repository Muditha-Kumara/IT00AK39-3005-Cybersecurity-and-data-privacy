
# GDPR Compliance Checklist – Web-based Booking System

This checklist helps ensure your booking system meets GDPR requirements. Mark each item as completed when done.

---

## 1. Personal Data Mapping and Minimization

- [ ] All personal data collected and processed in the system is identified (e.g., name, email, age, username).
- [ ] Only necessary personal data is collected (data minimization).
- [ ] User age is recorded to verify booker is over 15 years old.

## 2. User Registration and Management

- [ ] Registration form includes GDPR-compliant consent for processing personal data (e.g., acceptance of privacy policy).
- [ ] Users can view, edit, and delete their own personal data via their account.
- [ ] Administrator can delete a reserver in accordance with the "right to be forgotten".
- [ ] Underage registration (under 15 years) and booking functionality is restricted.

## 3. Booking Visibility

- [ ] Bookings are visible to non-logged-in users only at the resource level (no personal data shown).
- [ ] Names, emails, or other personal data of bookers are not exposed publicly or to unauthorized users.

## 4. Access Control and Authorization

- [ ] Only administrators can add, modify, and delete resources and bookings.
- [ ] System uses role-based access control (e.g., reserver vs. administrator).
- [ ] Administrator privileges are limited to ensure GDPR compliance (e.g., no unauthorized use of data).

## 5. Privacy by Design Principles

- [ ] Privacy by Default is implemented (minimum data collected by default).
- [ ] Logs are implemented without unnecessarily storing personal data.
- [ ] Forms and system components are designed with data protection in mind (e.g., secured login, minimal fields).

---

_Last updated: December 19, 2025_
---

| **Result** | **Data security** |
| :----: | :--- |
| &nbsp;✅/❌/⚠️&nbsp; | Are CSRF, XSS, and SQL injection protections implemented? |
| &nbsp;✅/❌/⚠️&nbsp; | Are passwords securely hashed using a strong algorithm (e.g., bcrypt, Argon2)? |
| &nbsp;/⚠️&nbsp; | Are data backup and recovery processes GDPR-compliant? |
| &nbsp;/⚠️&nbsp; | Is personal data stored in data centers located within the EU? |

---

| **Result** | **Data anonymization and pseudonymization** |
| :----: | :--- |
| &nbsp;⚠️&nbsp; | Is personal data anonymized where possible? |
| &nbsp;✅/❌/⚠️&nbsp; | Are pseudonymization techniques used to protect data while maintaining its utility? |

---

| **Result** | **Data subject rights** |
| :----: | :--- |
| &nbsp;⚠️&nbsp; | Can users download or request all personal data related to them (data access request)? |
| &nbsp;⚠️&nbsp; | Is there an interface or process for users to request the deletion of their personal data? |
| &nbsp;⚠️&nbsp; | Can users withdraw their consent for data processing? |

---

| **Result** | **Documentation and communication** |
| :----: | :--- |
| &nbsp;⚠️&nbsp; | Is there a privacy policy available to users during registration and easily accessible? |
| &nbsp;⚠️&nbsp; | Are administrators and developers provided with documented data protection practices <br>and processing activities? |
| &nbsp;❌&nbsp; | Is there a documented data breach response process (e.g., how to notify authorities <br>and users of a breach)? |

---
## Overall Assessment
⚠️  The system is **partially GDPR compliant**.
Improvements are needed in documentation, user rights automation, and log retention.