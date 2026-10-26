# Advanced SQL Injection

{% embed url="https://tryhackme.com/room/advancedsqlinjection" %}

Advanced SQL Injection techniques beyond classic in-band queries: Second-Order SQLi, WAF/Filter evasion, Out-of-Band (OOB) exfiltration over HTTP/DNS, and automated SQLmap tamper scripts.

---

## 1. Classification of SQL Injection Channels

- **In-band SQLi:** Uses the same primary communication channel to inject payloads and retrieve database results (UNION-based, error-based).
- **Inferential (Blind) SQLi:** Reconstructs data bit-by-bit by observing boolean responses or time delays (`pg_sleep()`, `WAITFOR DELAY`).
- **Out-of-Band (OOB) SQLi:** Used when direct responses are completely blind and time delays are unstable. Forces the DBMS to initiate external network requests (via **HTTP** or **DNS**) to an attacker-controlled listener containing extracted data.

---

## 2. Second-Order SQL Injection

Occurs when input is stored safely in the database during Step 1 (e.g. creating user `admin'--`), but is later concatenated into a secondary query during Step 2 (e.g. password resets or admin logs), triggering SQL execution.

---

## 3. Filter Evasion & Tampering

Bypassing basic regex filters and Web Application Firewalls:
- **Comment Injection:** `SEL/**/ECT` or `UN/**/ION`
- **Case Alternation:** `uNiOn sElEcT`
- **Encoding:** Double URL encoding (`%2527`) or hexadecimal literals.

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 1** | SQLi using the same channel for injection and retrieval | `In-band` |
| **Task 2** | Protocol commonly used in Out-of-Band SQLi exfiltration | `HTTP` |
| **Concept** | Injection where payload executes in a secondary subsequent query | Second-Order SQL Injection |
| **Concept** | Tool utilized for automated advanced SQLi tamper scripts | `sqlmap` |
