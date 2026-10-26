# Advent of Cyber 2024 - Day 23: Hash Cracking

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

You wanna know something? Hashes don't lie.

## Background

Cryptographic hash functions produce fixed-length digests from arbitrary input data. Password hashes are cracked offline using dictionary attacks, rule-based mutations, and brute-force cracking.

Hashcat leverages GPU parallelism to test millions or billions of candidate passwords per second. Identifying the exact hash format (`hashid`, `hash-identifier`) is the essential first step before launching a cracking session.

## Walkthrough

**Step 1 — Identify the Hash Format**

Examine the target hash file:
```bash
hashid target_hash.txt
```
The digest matches raw MD5 (Hashcat mode `-m 0`).

**Step 2 — Run Hashcat Against Wordlist**

Launch Hashcat with `rockyou.txt`:
```bash
hashcat -m 0 target_hash.txt /usr/share/wordlists/rockyou.txt
```
Hashcat cracks the digest, revealing plaintext: `fluffycat12`.

**Step 3 — Decrypt the Archive**

Unlock the password-protected zip archive using `fluffycat12` to extract `flag.txt`:
```bash
unzip -P fluffycat12 secret_archive.zip
cat flag.txt
```
Flag recovered: `THM{do_not_GET_CAUGHT}`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the cracked plaintext password? | `fluffycat12` |
| What is the flag retrieved from the decrypted archive? | `THM{do_not_GET_CAUGHT}` |
