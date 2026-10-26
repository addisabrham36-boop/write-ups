# Advent of Cyber 2022 - Day 16: Secure Coding

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Analyzing PHP source code for SQL injection vulnerabilities and implementing remediation using prepared statements and type-casting.

## Walkthrough
1. Inspect the vulnerable PHP snippet:
   ```php
   $id = $_GET['id'];
   $query = "SELECT * FROM toys WHERE id = " . $id;
   ```
2. Exploiting via UNION query injection:
   ```sql
   1 UNION SELECT 1, flag, 3 FROM secrets-- -
   ```
3. Secure remediation:
   - For integer inputs, enforce strict integer casting: `$id = intval($_GET['id']);`
   - For general queries, use PDO prepared statements with parameterized inputs.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Function used to enforce integer types in PHP? | `intval` |
| Code review completion flag | `THM{SQLI_PREVENTED_SUCCESS}` |
