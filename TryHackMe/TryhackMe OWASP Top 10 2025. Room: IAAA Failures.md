# TryHackMe OWASP Top 10 2025: IAAA Failures

{% embed url="https://tryhackme.com/room/owasptop102025iaaafailures" %}

Comprehensive breakdown of Identity, Authentication, Authorisation, and Accountability (IAAA) failures from the OWASP Top 10 2025 update: Broken Access Control (A01), Authentication Failures (A07), and Logging & Alerting Failures (A09).

---

## 1. What is IAAA?

IAAA defines the identity and governance boundary in web architecture:
- **Identity:** Identifying who a user claims to be.
- **Authentication:** Verifying proof of identity.
- **Authorisation:** Determining resource permissions.
- **Accountability:** Logging and attributing user actions.

---

## 2. A01: Broken Access Control (Horizontal PrivEsc)

Accessing other accounts at the same permission tier is **Horizontal** privilege escalation.
- Intercepting profile view parameters and cycling user records until locating the user with a balance exceeding $1,000,000 reveals their private note:
  `THM{Found.the.Millionare!}`

---

## 3. A07: Authentication Failures

Exploiting account confusion and flawed session handling on the administrative portal bypasses login controls and unlocks the admin dashboard flag:
`THM{Account.confusion.FTW!}`

---

## 4. A09: Logging & Alerting Failures

Auditing application authentication logs:
- A high volume of failed authentication events reveals a targeted password brute-force attack originating from external IP:
  `203.0.113.45`

---

## 🏁 Questions & Answers

| Task | Category | Question | Answer |
|---|---|---|---|
| **Task 2** | Concept | What does IAAA stand for? | `Identity, Authentication, Authorisation, Accountability` |
| **Task 3** | A01 | Privilege escalation viewing another user's data at same level | `Horizontal` |
| **Task 3** | A01 | Note found on account with >$1 million | `THM{Found.the.Millionare!}` |
| **Task 4** | A07 | Flag on the admin user's dashboard | `THM{Account.confusion.FTW!}` |
| **Task 5** | A09 | IP address of brute-force attacker | `203.0.113.45` |
