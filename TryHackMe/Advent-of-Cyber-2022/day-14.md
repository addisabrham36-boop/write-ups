# Advent of Cyber 2022 - Day 14: Web Applications

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Exploiting **Insecure Direct Object References (IDOR)** within an internal corporate directory application.

## Walkthrough
1. Log into the staff portal with provided credentials: `mcskidy:devtest`.
2. Inspect the user profile URL:
   ```text
   http://TARGET_IP/profile.php?id=101
   ```
3. Modify the `id` parameter sequentially (`102`, `103`, `104`, etc.) or fuzz with Burp Intruder.
4. At `id=105`, we locate the profile of **Elf Pivot McRed**.
5. The profile displays personal metadata including role, emergency contact, and office number: **134**.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the office number of Elf Pivot McRed? | `134` |
