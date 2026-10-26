# OWASP Top 10 2025: A01 Broken Access Control

{% embed url="https://tryhackme.com/room/owasptop102025iaaafailures" %}

Analysis of OWASP A01: Broken Access Control within the 2025 IAAA (Identity, Authentication, Authorisation, Accountability) framework, focusing on horizontal vs vertical privilege escalation and IDOR.

---

## 1. What is IAAA?

IAAA defines the four pillars of access management:
- **Identity:** Declaring who a user or entity claims to be.
- **Authentication:** Verifying the declared identity (passwords, MFA, keys).
- **Authorisation:** Determining what resources and actions the authenticated identity is permitted to access.
- **Accountability:** Logging and tracing actions back to an identity (non-repudiation and auditing).

---

## 2. Broken Access Control Concepts

- **Vertical Privilege Escalation:** An unprivileged user accesses functions or data reserved for higher privilege tiers (e.g. standard user performing admin operations).
- **Horizontal Privilege Escalation:** A user accesses functions or private data of another user possessing the exact same privilege tier (e.g. user A viewing user B's account balance via IDOR).

---

## 3. IDOR Exploitation Walkthrough

In the vulnerable web application, users view their balance through:
```http
GET /api/account?id=104 HTTP/1.1
```

By altering the `id` parameter sequentially or testing different object references, we access another user's profile without authorization checks:
- Intercept the request in Burp Suite.
- Increment the identifier to locate the account holding over $1,000,000.
- Retrieve the sensitive note stored in their account profile.

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | What does IAAA stand for? | `Identity, Authentication, Authorisation, Accountability` |
| **Task 3** | Accessing data of another user at the same level is what type of privesc? | `horizontal privilege escalation` |
| **Task 3** | Note found viewing the user account with over $1 million | Retrieved via IDOR by incrementing target account IDs |
