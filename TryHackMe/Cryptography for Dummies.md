# Cryptography for Dummies

{% embed url="https://tryhackme.com/room/cryptographyfordummies" %}

A beginner-friendly guide to cryptography fundamentals: symmetric vs asymmetric encryption, key exchange, hashing algorithms, MD5 history, and Base64 encoding.

---

## 1. Types of Cryptography

- **Symmetric Cryptography:** Uses a single shared key for both encryption and decryption (e.g. AES). It is computationally faster than asymmetric cryptography.
- **Asymmetric Cryptography:** Uses a mathematically linked key pair:
  - **Public Key:** Distributed openly to anyone who wishes to encrypt data.
  - **Private Key:** Kept strictly secret by the owner to decrypt data or sign documents.
  - More secure for open networks (e.g., banking portals, TLS/HTTPS).

---

## 2. Cryptographic Hashes

A hash function is a one-way mathematical algorithm converting arbitrary-length input into a fixed-length string. Hashes cannot be reversed by design.
- **MD5:** Message Digest 5, created by cryptographer **Ronald Rivest** in 1991.
- Computing MD5 for `hashes are cool`:
  ```bash
  echo -n "hashes are cool" | md5sum
  # f762d32e3c160900d94b683e927555b9
  ```

---

## 3. Encoding vs Encryption

Base64 is a binary-to-text encoding scheme, **not** encryption:
```bash
echo -n "cryptographyisuseful" | base64
# Y3J5cHRvZ3JhcGh5aXN1c2VmdWw=

echo -n "dGhlIHNlY3JldCB3b3JkIGlzIDogd2F0ZXJtZWxvbg==" | base64 -d
# the secret word is : watermelon
```

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | What type of cryptography is more secure? | `Asymmetric` |
| **Task 2** | What type of cryptography is faster? | `Symmetric` |
| **Task 2** | What type of cryptography will a Bank site use? | `Asymmetric` |
| **Task 2** | Key used to encrypt messages in asymmetric cryptography | `public key` |
| **Task 2** | Key used to decrypt messages in asymmetric cryptography | `private key` |
| **Task 2** | Does symmetric cryptography use two different keys? (yea/nay) | `nay` |
| **Task 3** | MD5 hash of "hashes are cool" | `f762d32e3c160900d94b683e927555b9` |
| **Task 3** | What does MD5 stand for? | `message digest 5` |
| **Task 3** | Who created MD5? | `Ronald Rivest` |
| **Task 4** | Encode "cryptographyisuseful" with Base64 | `Y3J5cHRvZ3JhcGh5aXN1c2VmdWw=` |
| **Task 4** | Secret word decoded from Base64 | `watermelon` |
