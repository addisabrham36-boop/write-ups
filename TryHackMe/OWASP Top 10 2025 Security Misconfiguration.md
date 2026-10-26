# OWASP Top 10 2025: Security Misconfiguration

{% embed url="https://tryhackme.com/room/owasptop102025applicationdesignflaws" %}

Walkthrough of Security Misconfigurations (AS02) in the OWASP Top 10 2025 series, focusing on unhandled input exceptions, verbose debugging disclosures, and API error leaks.

---

## 1. Vulnerability Overview

Security Misconfiguration occurs when components are deployed with default credentials, unnecessary features enabled, permissive headers, or verbose error logging exposed directly to end-users.

---

## 2. Exploiting Unhandled Verbose Errors

The target User Management API runs on port `5002`. It expects integer parameters for user records:

```http
GET /api/user/1 HTTP/1.1
Host: MACHINE_IP:5002
```

When passing an unexpected alphanumeric string instead of an integer:

```bash
curl http://MACHINE_IP:5002/api/user/admin
```

Because error handling is not centrally sanitized and debug mode remains enabled in production:
1. The backend throws an unhandled casting exception.
2. The complete stack trace and execution context are returned in the response.
3. The environment variables embedded in the error disclose the hidden flag:
   `THM{V3RB0S3_3RR0R_L34K}`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **AS02** | Flag extracted from verbose error leak on port 5002 | `THM{V3RB0S3_3RR0R_L34K}` |
| **Concept** | Best defensive remediation | Disable detailed error/stack traces in production and implement centralized generic error handlers |
