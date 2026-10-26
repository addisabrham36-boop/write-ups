# Introduction to Cryptography Basics

{% embed url="https://tryhackme.com/room/cryptographybasics" %}

A foundational walkthrough of fundamental cryptographic mechanisms: PCI DSS compliance, plaintext vs ciphertext, Caesar shift ciphers, DES vs AES standards, XOR arithmetic, and modular math.

---

## 1. Cryptographic Principles & Standards

- **Ciphertext:** Encrypted plaintext output produced by applying an encryption algorithm with a key.
- **Decryption:** The mathematical process reversing ciphertext back into readable plaintext.
- **PCI DSS:** Payment Card Industry Data Security Standard governing credit card encryption and transmission.

---

## 2. Historical & Modern Ciphers

- **Caesar Cipher:** Symmetric substitution shifting letters along the alphabet:
  - Decrypting `XRPCTCRGNEI` (shift of 15):
    `ICANENCRYPT`
- **DES (Data Encryption Standard):** 56-bit key length is now considered broken and insecure (**Nay**).
- **AES (Advanced Encryption Standard):** Standardized by NIST in **2001** (FIPS 197), supporting 128, 192, and 256-bit keys.

---

## 3. Cryptographic Mathematics

- **Binary XOR (`⊕`):**
  ```text
    1 0 0 1
  ⊕ 1 0 1 0
  ---------
    0 0 1 1
  ```
- **Modulo Arithmetic (`%`):** Returns the remainder of division:
  - `118613842 % 9091 = 3565`
  - `60 % 12 = 0`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | Standard required for handling credit card information | `PCI DSS` |
| **Task 3** | What do you call the encrypted plaintext? | `ciphertext` |
| **Task 3** | Process that returns the plaintext | `decryption` |
| **Task 4** | Plaintext of Caesar cipher `XRPCTCRGNEI` | `ICANENCRYPT` |
| **Task 5** | Should you trust DES? (Yea/Nay) | `Nay` |
| **Task 5** | When was AES adopted as an encryption standard? | `2001` |
| **Task 6** | What's `1001 ⊕ 1010`? | `0011` |
| **Task 6** | What's `118613842 % 9091`? | `3565` |
| **Task 6** | What's `60 % 12`? | `0` |
