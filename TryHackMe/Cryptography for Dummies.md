# Cryptography for Dummies: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/cryptographyfordummies

## Room Overview & Information

**Category:** Cryptography fundamentals  
**Difficulty:** Beginner  
**Focus:** Symmetric and asymmetric cryptography, public/private keys, MD5 hashing, Base64 encoding, and decoding.

Cryptography protects information through different mechanisms. Encryption uses a key to protect confidentiality, hashing creates a one-way digest, and encoding changes representation. These are related but distinct operations.

## Task 1: Introduction

Read the room's introduction and open the practical where required. The tasks ask you to compare cryptography types and perform simple hash and Base64 operations.

## Task 2: Types of Cryptography

### Question: “What type of cryptography is more secure?”

**Room answer:** `asymmetric`.

The room contrasts symmetric and asymmetric cryptography. Asymmetric systems use a related public/private key pair and can support authenticated key exchange or signatures. Security depends on the algorithm, key size, implementation, and how trust is established; asymmetric cryptography is not universally “more secure” for every use case.

### Question: “What type of cryptography is faster?”

**Answer:** `symmetric`.

Symmetric algorithms are generally faster for encrypting large amounts of data. Modern protocols often use asymmetric techniques to establish or authenticate keys, then use symmetric encryption for the bulk data.

### Question: “What type of cryptography will a Bank site use?”

**Room answer:** `asymmetric`.

The expected answer refers to the public-key cryptography used by HTTPS/TLS to authenticate a server and establish secure communication. In practice, TLS combines asymmetric cryptographic mechanisms with symmetric session encryption.

### Question: “What will you use to encrypt your messages in asymmetric cryptography?”

**Answer:** `public key`.

In a basic public-key encryption model, a sender encrypts a message using the recipient's public key. Only the matching private key should be able to decrypt it.

### Question: “What will you use to decrypt messages in asymmetric cryptography?”

**Answer:** `private key`.

The private key must remain secret. In a signature system, the roles differ: the sender signs with a private key and others verify with the public key.

### Question: “Does symmetric cryptography use two different keys for encryption/decryption? (aye/nay)”

**Answer:** `nay`.

Symmetric encryption uses the same shared secret key, or an equivalent shared secret, for encryption and decryption.

## Task 3: Hashing

A cryptographic hash maps input data to a fixed-length digest. It is not designed to be decrypted. The room uses MD5 for a simple example, although MD5 is no longer suitable for security-sensitive integrity guarantees because of known collision weaknesses.

### Question: “What's the MD5 hash of "hashes are cool"?”

**Answer:** `f762d32e3c160900d94b683e927555b9`.

Verify it locally:

\`\`\`bash
printf '%s' 'hashes are cool' | md5sum
\`\`\`

Use `printf` to avoid adding a newline to the input. An extra newline changes the resulting hash.

### Question: “What does MD5 stand for?”

**Answer:** **Message Digest 5**.

### Question: “Who created MD5?”

**Answer:** **Ronald Rivest**.

MD5 remains useful as a legacy identifier in some datasets, but use SHA-256 or a suitable modern alternative for new integrity-checking systems. For password storage, use a password-hashing scheme such as Argon2id, scrypt, or bcrypt rather than a fast hash.

## Task 4: Encoding

Base64 converts bytes into a printable character representation. It is encoding, not encryption, and it does not protect the data from someone who can read it.

### Question: “Encode the string "cryptographyisuseful" with Base64”

**Answer:** `Y3J5cHRvZ3JhcGh5aXN1c2VmdWw=`.

Verify it with:

\`\`\`bash
printf '%s' 'cryptographyisuseful' | base64
\`\`\`

### Question: “Decode the string "dGhlIHNlY3JldCB3b3JkIGlzIDogd2F0ZXJtZWxvbg==". What's the secret word?”

Decode it with:

\`\`\`bash
printf '%s' 'dGhlIHNlY3JldCB3b3JkIGlzIDogd2F0ZXJtZWxvbg==' | base64 --decode
\`\`\`

**Answer:** `watermelon`.

The decoded text says the secret word is watermelon. Base64 can be reversed without a key; do not use it to protect passwords or confidential data.

## Task 5: Practical distinctions

| Operation | Purpose | Requires a secret key? | Reversible? |
|---|---|---|---|
| Symmetric encryption | Confidentiality | Yes | With the correct key |
| Asymmetric encryption | Confidentiality / key exchange | Public/private key pair | With the matching private key |
| Hashing | Digest / integrity comparison | Usually no | Not designed to be reversed |
| Base64 encoding | Data representation | No | Yes |

## Key takeaways

Symmetric cryptography is generally faster; asymmetric cryptography supports public/private key operations; hashes are not encryption; and Base64 is only encoding. Use modern algorithms, protect private keys, and avoid MD5 for security-sensitive applications.