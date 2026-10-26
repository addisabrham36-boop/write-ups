# Cryptography for Dummies

Official room: https://tryhackme.com/room/cryptographyfordummies

## Room Overview & Information

**Category:** Cryptography fundamentals  
**Difficulty:** Beginner  
**Topics:** Symmetric and asymmetric cryptography, public/private keys, hashing, MD5, and Base64 encoding.

This is a learning room rather than a boot-to-root challenge. Port scanning, initial access, and privilege escalation are not applicable. The practical work is to classify the operation being used, apply a simple encoding or digest command, and verify the exact output.

## Phase 1: Reconnaissance & Enumeration of the Task

Read each question carefully and decide which property it describes. Confidentiality protects information from unauthorized reading; integrity helps detect modification; authentication establishes identity or origin within a trust model; encoding changes representation; hashing produces a digest; and encryption uses keys to protect data.

The fastest way to make a mistake is to treat encoding, hashing, and encryption as interchangeable terms. They are not.

## Phase 2: Cryptography Fundamentals

### Task 1: Introduction

The room introduces basic cryptographic goals and a small practical exercise. Use the wording of each question to select the mechanism being asked about.

### Task 2: Types of Cryptography

**Question:** “What type of cryptography is more secure?”

**Room answer:** asymmetric.

This is the expected answer in the room's comparison, but “more secure” depends on the use case. Asymmetric methods use a related public/private key pair and can support key exchange and signatures. Symmetric algorithms are also secure when correctly selected and implemented.

**Question:** “What type of cryptography is faster?”

**Answer:** symmetric.

Symmetric algorithms are generally faster for encrypting large amounts of data. Modern protocols often use asymmetric operations to authenticate parties or establish keys, then use symmetric encryption for the bulk traffic.

**Question:** “What type of cryptography will a Bank site use?”

**Room answer:** asymmetric.

The expected concept is the public-key cryptography involved in HTTPS/TLS. In practice, TLS combines certificates and public-key mechanisms with symmetric session encryption rather than relying on asymmetric encryption for every byte.

**Question:** “What will you use to encrypt your messages in asymmetric cryptography?”

**Answer:** public key.

In the simple public-key encryption model, the sender encrypts using the recipient's public key.

**Question:** “What will you use to decrypt messages in asymmetric cryptography?”

**Answer:** private key.

The matching private key must be protected. Signature systems use the key pair differently: the sender signs with the private key and verifiers use the public key.

**Question:** “Does symmetric cryptography use two different keys for encryption/decryption? (aye/nay)”

**Answer:** nay.

Symmetric encryption uses the same shared secret key, or an equivalent shared secret, for encryption and decryption.

## Phase 3: Hashing and Encoding Practicals

### Task 3: Hashing

A cryptographic hash maps input bytes to a fixed-length digest. A hash is not normally decrypted. MD5 is used in this beginner exercise, but it is no longer suitable for security-sensitive integrity protection because collision attacks are practical.

**Question:** “What's the MD5 hash of "hashes are cool"?”

**Answer:** `f762d32e3c160900d94b683e927555b9`.

Verify it locally:

```bash
printf '%s' 'hashes are cool' | md5sum
```

The `printf` command does not add a newline. If you use `echo` instead, a newline may be included and the digest will differ.

**Question:** “What does MD5 stand for?”

**Answer:** Message Digest 5.

**Question:** “Who created MD5?”

**Answer:** Ronald Rivest.

### Task 4: Encoding

Base64 turns bytes into a printable text representation. It does not encrypt or protect the data.

**Question:** “Encode the string "cryptographyisuseful" with Base64”

**Answer:** `Y3J5cHRvZ3JhcGh5aXN1c2VmdWw=`.

Verify:

```bash
printf '%s' 'cryptographyisuseful' | base64
```

**Question:** “Decode the string "dGhlIHNlY3JldCB3b3JkIGlzIDogd2F0ZXJtZWxvbg==". What's the secret word?”

Use:

```bash
printf '%s' 'dGhlIHNlY3JldCB3b3JkIGlzIDogd2F0ZXJtZWxvbg==' | base64 --decode
```

**Answer:** `watermelon`.

The output reveals the secret word. Base64 can be reversed without a key; do not use it to protect passwords or confidential values.

### Task 5: Distinguish the operations

| Operation | Purpose | Key required? | Reversible? |
|---|---|---|---|
| Symmetric encryption | Confidentiality | Shared secret | Yes, with the key |
| Asymmetric encryption | Confidentiality or key establishment | Public/private pair | With matching private key |
| Hashing | Digest and integrity comparison | Usually no | Not designed to reverse |
| Base64 encoding | Data representation | No | Yes |

## Phase 4: Room Questions & Answer Summary

| Question | Answer |
|---|---|
| What type of cryptography is more secure? | asymmetric (the room's intended comparison) |
| What type of cryptography is faster? | symmetric |
| What type of cryptography will a Bank site use? | asymmetric (TLS uses both asymmetric and symmetric mechanisms) |
| What encrypts a message in asymmetric cryptography? | public key |
| What decrypts the message? | private key |
| Does symmetric cryptography use two different keys? | nay |
| MD5 hash of “hashes are cool” | `f762d32e3c160900d94b683e927555b9` |
| What does MD5 stand for? | Message Digest 5 |
| Who created MD5? | Ronald Rivest |
| Base64 of “cryptographyisuseful” | `Y3J5cHRvZ3JhcGh5aXN1c2VmdWw=` |
| Secret word decoded from Base64 | `watermelon` |
