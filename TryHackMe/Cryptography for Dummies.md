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

## Expanded cryptography walkthrough

### 1. Identify the security goal

Cryptography provides several different properties. Confidentiality prevents unauthorized reading; integrity detects unauthorized modification; authentication helps verify who or what sent a message; and digital signatures provide integrity and origin evidence under a trust model. Encoding and hashing are not encryption, despite frequent human efforts to make terminology optional.

### 2. Distinguish encoding, hashing, and encryption

- **Encoding** changes representation so data can be transported or displayed. Base64 is reversible and provides no secrecy.
- **Hashing** maps data to a fixed-length digest. A secure hash is designed to make it impractical to recover the input or find collisions, but it is not decrypted.
- **Symmetric encryption** uses a shared secret key to encrypt and decrypt data.
- **Asymmetric cryptography** uses related public and private keys. It supports key agreement, encryption in some schemes, and digital signatures.
- **Digital signatures** are verified with a public key; they do not make the signed content confidential.

### 3. Inspect common encodings

For a Base64 string supplied by the room, decode it with:

```bash
printf '%s' 'BASE64_VALUE' | base64 --decode
```

Use `printf` to avoid adding a newline to the encoded input. If decoding fails, check padding and whether the value is actually Base64. Hexadecimal can be inspected with `xxd -r -p`, but only when the data is truly hex-encoded.

Do not keep decoding blindly. Identify the likely format from the alphabet, padding, output structure, and task clues. A successful decode can still produce another encoded layer or binary data.

### 4. Understand weak password hashes

Password storage should use a dedicated password-hashing scheme such as Argon2id, scrypt, or bcrypt with appropriate parameters and salts. Fast general-purpose hashes such as SHA-256 are unsuitable for direct password storage because attackers can test guesses rapidly.

In a controlled lab, identify the hash format from its structure and use the room's provided wordlist or cracking tool. A candidate match is a hypothesis until it is validated in the intended task. Never test recovered credentials against systems outside the lab.

### 5. Avoid common cryptographic mistakes

Do not invent your own cipher. Use authenticated encryption, such as a well-reviewed AEAD mode, rather than encryption without integrity protection. Manage keys separately from ciphertext, rotate exposed keys, and do not hard-code secrets in public source repositories. A long key is not useful if it is stored next to the encrypted data or logged in plaintext.

### 6. Complete the room tasks

For each question, determine whether it asks about encoding, hashing, encryption, key exchange, or signatures. Use the method that matches the requested property. Record the transformation and check the result's structure. If the task provides a particular ciphertext or hash, use that exact value rather than an example from another guide.

### 7. Defensive takeaway

Cryptography is only as strong as the algorithm, parameters, key handling, and surrounding implementation. Correctly naming a primitive matters, but secure systems also require authenticated protocols, sound randomness, protected keys, and clear separation between encoding and encryption.
