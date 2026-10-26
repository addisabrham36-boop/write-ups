# Enumeration & Brute Force

{% embed url="https://tryhackme.com/room/enumerationbruteforce" %}

Web authentication enumeration and brute force fundamentals. Multiple small labs under `enum.thm` covering verbose errors, password reset flaws, HTTP Basic Auth, and Burp Intruder.

Add the target to `/etc/hosts`:

```bash
echo "MACHINE_IP enum.thm" | sudo tee -a /etc/hosts
```

---

## Task 2 — Authentication Enumeration

The labs at `http://enum.thm/labs/verbose_login/` show how login forms leak account existence. Submit an unknown email and a known email with a wrong password — compare the error messages.

**Q: What type of error messages can unintentionally confirm valid usernames?**
**A: `Verbose errors`**

**Q: Which feature, if poorly implemented, can assist attackers via response differences?**
**A: `Password reset`**

---

## Task 3 — Enumerating Users via Verbose Errors

The lab returns different messages for unknown vs. existing emails. Use Burp Intruder or a script with the provided username list to find the valid one.

**Q: What is the valid email from the list?**
**A: `canderson@gmail.com`**

---

## Task 4 — Predictable Password Reset Tokens

Inspect the reset token in the email link — if it's a timestamp or sequential value, it's predictable. Generate the right token offline and use it to take over another account.

---

## Task 5 — HTTP Basic Authentication Brute Force

The lab at `/labs/basic_auth/` requires credentials over HTTP Basic Auth. Brute force with Hydra:

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt \
  enum.thm http-get /labs/basic_auth/
```

Or with Burp Intruder on the `Authorization: Basic` header (base64 encode `user:pass` for each attempt).

---

## Task 6 — OSINT on Password Reset

The reset question uses a field that could be answered from public OSINT (social media, GitHub profile, etc.). Research the target username to answer the security question and reset their password.

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What type of errors confirm valid usernames? | `Verbose errors` |
| Which feature leaks usernames via response differences? | `Password reset` |
| Valid email from the list | `canderson@gmail.com` |
