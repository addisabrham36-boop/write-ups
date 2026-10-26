# OWASP Top 10 2025: Application Design Flaws

{% embed url="https://tryhackme.com/room/owasptopten2025two" %}

Hands-on labs covering modern architecture and design flaws from the OWASP Top 10 2025 series: Security Misconfigurations (AS02), Software Supply Chain Failures (AS03), Cryptographic Failures (AS04), and Insecure Design (AS06).

---

## 1. Challenge Architecture & Ports

The target environment hosts independent microservices on distinct local ports:
- **Port 5002 (AS02 - Security Misconfiguration):** User Management API with verbose error debugging.
- **Port 5003 (AS03 - Software Supply Chain Failures):** Vulnerable third-party utility component imports.
- **Port 5004 (AS04 - Cryptographic Failures):** Reversible/weak cryptographic key generation.
- **Port 5005 (AS06 - Insecure Design):** Flawed business logic relying on client-side headers.

---

## 2. Walkthrough & Exploitation

### AS02 — Security Misconfiguration (Port 5002)
Triggering unhandled exceptions on `/api/user/<id>` returns internal environment variables leaking:
`flag{hidden}`

### AS06 — Insecure Design (Port 5005)
The application assumes administrative or privileged capabilities are restricted exclusively to mobile clients, relying purely on the `User-Agent` HTTP header for authorization:
```bash
curl -H "User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 16_0 like Mac OS X)" http://MACHINE_IP:5005/admin
```
Bypassing the naive design restriction grants administrative access to the flag.

---

## 🏁 Questions & Answers

| Task | Category | Answer / Finding |
|---|---|---|
| **Task 2** | AS02 - Security Misconfiguration | `flag{hidden}` |
| **Task 3** | AS03 - Supply Chain Failures | Outdated third-party utility dependency |
| **Task 4** | AS04 - Cryptographic Failures | Decrypted using recovered weak cipher key |
| **Task 5** | AS06 - Insecure Design | Bypassed mobile-only restriction via `User-Agent` spoofing |
