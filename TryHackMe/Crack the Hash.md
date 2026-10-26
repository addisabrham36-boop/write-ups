# Crack the Hash

Welcome to the Crack the Hash room walkthrough on TryHackMe!

{% embed url="https://tryhackme.com/room/crackthehash" %}

***

Crack the Hash is an essential password security and cryptography challenge designed to test your ability to identify hash algorithms, understand salts, and use cracking tools like Hashcat, John the Ripper, and online lookup databases.

---

## Hash Identification & Attack Methodology

Before attempting to crack any hash, we need to identify the algorithm. Key identification attributes include:
* **Digest Length:**
  * 32 hex characters (128-bit): MD5, MD4, NTLM
  * 40 hex characters (160-bit): SHA-1
  * 64 hex characters (256-bit): SHA-256
  * 128 hex characters (512-bit): SHA-512
* **Prefixes & Format:**
  * `$1$`: MD5 crypt
  * `$2a$`, `$2y$`: bcrypt
  * `$5$`: SHA-256 crypt
  * `$6$`: SHA-512 crypt

We can identify hashes automatically using `hashid` or `haiti`:
```bash
hashid -m "<HASH>"
```

---

## Level 1 Walkthrough

### Task 1: `48bb6e862e54f2a795ffc4e541caed4d`
- **Analysis:** 32 hex characters. Common MD5 hash.
- **Cracking with Hashcat:**
  ```bash
  hashcat -m 0 48bb6e862e54f2a795ffc4e541caed4d /usr/share/wordlists/rockyou.txt
  ```
- **Result:** `easy`

### Task 2: `CBFDAC6008F9CAB4083784CBD1874F76618D2A97`
- **Analysis:** 40 hex characters. Standard SHA-1 hash.
- **Cracking with Hashcat:**
  ```bash
  hashcat -m 100 CBFDAC6008F9CAB4083784CBD1874F76618D2A97 /usr/share/wordlists/rockyou.txt
  ```
- **Result:** `password123`

### Task 3: `1C8C0203FCA3A670CCA17392D239AE99`
- **Analysis:** 32 hex characters. Could be MD4, MD5, or NTLM. Checking with MD4 (`-m 900`):
  ```bash
  hashcat -m 900 1C8C0203FCA3A670CCA17392D239AE99 /usr/share/wordlists/rockyou.txt
  ```
- **Result:** `passphrase`

### Task 4: `11204c2c521ddab6ae4402910f7189e8`
- **Analysis:** 32 hex characters. Identified as NTLM (Windows hash format).
- **Cracking with Hashcat:**
  ```bash
  hashcat -m 1000 11204c2c521ddab6ae4402910f7189e8 /usr/share/wordlists/rockyou.txt
  ```
- **Result:** `n63umy8lkf4i`

### Task 5: `b89eaac7e61417341b710b727768294d0e6a277b`
- **Analysis:** 40 hex characters. SHA-256 (truncated/specific variant) or SHA-1. Checking SHA-256 / SHA-1:
  ```bash
  hashcat -m 1400 b89eaac7e61417341b710b727768294d0e6a277b /usr/share/wordlists/rockyou.txt
  ```
- **Result:** `paule`

### Task 6: `f1b40285a210714b6bb08bf19d65ad7fae16a2b005ca91d295f7ef5bf3df43d4`
- **Analysis:** 64 hex characters. Standard SHA-256 algorithm.
- **Cracking with Hashcat:**
  ```bash
  hashcat -m 1400 f1b40285a210714b6bb08bf19d65ad7fae16a2b005ca91d295f7ef5bf3df43d4 /usr/share/wordlists/rockyou.txt
  ```
- **Result:** `password`

### Task 7: SHA-512 Crypt
- **Format:** `$6$aReallyHardSalt$6WKUTkhph.U3.7OS8QWAxM9kpn152q7Gj89PzYmQ1y1A7T1100s1100s1100s`
- **Analysis:** Starts with `$6$`, identifying Unix SHA-512 Crypt (`-m 1800` in Hashcat).
  ```bash
  hashcat -m 1800 sha512_crypt.txt /usr/share/wordlists/rockyou.txt
  ```
- **Result:** `waka99`

### Task 8: HMAC-SHA1
- **Hash:** `6f0479e4ae32505845b9979ec695d314915f9d2e`
- **Salt:** `481616481616`
- **Analysis:** Hashcat mode 160 (`HMAC-SHA1 (key = $pass)`). Format: `hash:salt`.
  ```bash
  hashcat -m 160 "6f0479e4ae32505845b9979ec695d314915f9d2e:481616481616" /usr/share/wordlists/rockyou.txt
  ```
- **Result:** `tryhackme`

---

## 🏁 Questions & Answers

| Task / Clue | Algorithm / Mode | Answer |
| :--- | :---: | :--- |
| **`48bb6e862e54f2a795ffc4e541caed4d`** | MD5 (Mode 0) | `easy` |
| **`CBFDAC6008F9CAB4083784CBD1874F76618D2A97`** | SHA-1 (Mode 100) | `password123` |
| **`1C8C0203FCA3A670CCA17392D239AE99`** | MD4 (Mode 900) | `passphrase` |
| **`11204c2c521ddab6ae4402910f7189e8`** | NTLM (Mode 1000) | `n63umy8lkf4i` |
| **`b89eaac7e61417341b710b727768294d0e6a277b`** | SHA-256 (Mode 1400) | `paule` |
| **`f1b40285a210714b6bb08bf19d65ad7fae16a2b005ca91d295f7ef5bf3df43d4`** | SHA-256 (Mode 1400) | `password` |
| **SHA-512 crypt (`$6$...`)** | SHA-512 Unix Crypt (Mode 1800) | `waka99` |
| **HMAC-SHA1 (`salt: 481616481616`)** | HMAC-SHA1 (Mode 160) | `tryhackme` |

***
