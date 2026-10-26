# Advent of Cyber 2022 - Day 17: Secure Coding

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Using Regular Expressions (RegEx) for input validation, sanitization, and parsing structured strings.

## Walkthrough
Test regex patterns against the dataset using `egrep`:
```bash
# Matching valid email addresses
egrep -E "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$" emails.txt

# Matching internal domain records
egrep -c "\.santaclaus\.thm" domains.txt
```
Applying the correct regex filtering patterns isolates valid inputs from adversarial injection strings.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Total valid domains matched by the filter? | *(Extracted from challenge lab)* |
