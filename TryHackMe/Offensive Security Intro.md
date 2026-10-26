# Offensive Security Intro

{% embed url="https://tryhackme.com/room/offensivesecurityintro" %}

An introduction to the philosophy of offensive security and ethical hacking, walking through web enumeration with `dirb` and manipulating web transactions on the FakeBank application.

---

## 1. What is Offensive Security?

Offensive security involves actively testing systems, networks, and applications using the same tools and tactics employed by malicious adversaries to uncover and remediate vulnerabilities before they can be exploited.

---

## 2. Web Reconnaissance (FakeBank Lab)

1. Connect to the provided lab machine at `http://fakebank.thm`.
2. Inspect the dashboard: the user's account number is displayed as **`8881`**.
3. Run directory brute forcing with `dirb` to find hidden administrative or transfer routes:
   ```bash
   dirb http://fakebank.thm
   ```
4. In addition to `/images`, `dirb` discovers:
   `http://fakebank.thm/bank-transfer`

---

## 3. Modifying Balances

1. Navigate to `http://fakebank.thm/bank-transfer`.
2. Enter your account number (`8881`) and transfer a positive deposit amount (e.g. `$2000`).
3. Return to the account overview page. Once the balance flips positive, an alert banner renders the victory key:
   `BANK-HACKED`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 1** | Term describing simulating a hacker's actions to find weaknesses | `Offensive Security` |
| **Task 2** | Account number in the FakeBank application | `8881` |
| **Task 3** | Hidden URL discovered by dirb besides `/images` | `http://fakebank.thm/bank-transfer` |
| **Task 4** | Words in green text when balance turns positive (ALL CAPS) | `BANK-HACKED` |
