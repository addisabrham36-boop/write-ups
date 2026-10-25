# Introduction to Cryptography Basics: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/cryptographybasics

## Room Overview & Information

**Category:** Cryptography fundamentals  
**Difficulty:** Beginner  
**Focus:** Encoding, encryption, hashing, symmetric and asymmetric cryptography, keys, and signatures.

Cryptography helps protect confidentiality, integrity, and authenticity. The algorithms matter, but so do the keys, modes, implementation, and trust model. Strong cryptography can still fail if a key is hard-coded, a nonce is reused where uniqueness is required, or the application exposes plaintext before encryption.

## Phase 1: Separate the core concepts

**Encoding** changes representation so data can be transported or interpreted. It does not require a secret key.

**Encryption** transforms plaintext into ciphertext using an algorithm and key. Decryption requires the appropriate key or key pair.

**Hashing** maps input to a fixed-length digest. A cryptographic hash is designed to be one-way, so it is not “decrypted.”

**Digital signatures** allow a verifier to check that data was signed by the holder of a private key and has not changed since signing, assuming the key and trust process are valid.

These concepts can appear together in one protocol, but they solve different problems.

## Phase 2: Symmetric encryption

Symmetric encryption uses the same secret key, or an equivalent shared secret, for encryption and decryption. It is efficient for large data sets, but both parties need a secure way to obtain the key.

Modern authenticated-encryption schemes such as AES-GCM and ChaCha20-Poly1305 can protect confidentiality and detect tampering when used correctly. Nonces or IVs must follow the algorithm's requirements; misuse can seriously weaken security.

Do not create a custom encryption algorithm. Use a trusted library and follow the library's guidance for key generation, nonce management, and authentication.

## Phase 3: Asymmetric cryptography

Asymmetric cryptography uses a public/private key pair. The public key may be shared, while the private key must remain protected. Depending on the algorithm and protocol, public-key cryptography can support key exchange, encryption, or digital signatures.

SSH and TLS use public-key techniques in different ways. A public key's presence does not automatically prove who owns it; the trust model must bind that key to the intended person, server, or organisation.

## Phase 4: Hashing and password storage

A hash function produces a fixed-length digest. SHA-256 is widely used for integrity checks. MD5 and SHA-1 still appear in legacy datasets but should not be relied on for modern collision-resistant security.

Password storage is a special case. Passwords should be processed with a password-hashing scheme such as Argon2id, scrypt, or bcrypt, using unique salts and an appropriate work factor. A fast general-purpose hash makes offline guessing much cheaper.

For a file-integrity check:

```bash
sha256sum file.bin
```

Record the digest and compare it with a value obtained from a trusted source. A hash only tells you whether the bytes match the reference; it does not prove that the file itself is safe.

## Phase 5: Public-key trust and certificates

A TLS certificate binds a public key to a hostname or identity under a certificate authority's trust process. The client checks that the certificate is valid for the hostname and that the chain leads to a trusted authority. A self-signed certificate can still be used in a controlled lab, but the user must establish trust through another channel.

A valid HTTPS connection protects data in transit and helps authenticate the server. It does not prove that the website is honest or that its application is free from vulnerabilities.

## Phase 6: Practical analysis workflow

When a room presents an unfamiliar string or file:
1. Determine whether it is encoded, encrypted, compressed, or hashed.
2. Preserve the original bytes.
3. Inspect the file type and surrounding task context.
4. Identify the algorithm or format from evidence.
5. Use the intended decoder or decryption method.
6. If a key is required, obtain it from the authorised task rather than guessing.
7. Verify the resulting data and record the transformation.

Do not treat an unreadable string as proof of encryption. Base64, hex, and binary data can all appear opaque to a human reader.

## Phase 7: Common failure modes

- **Wrong key:** decryption fails or produces invalid data.
- **Wrong mode or parameters:** the algorithm is correct but the ciphertext cannot be interpreted.
- **Corrupted input:** whitespace or a missing byte changes the data.
- **Encoding confusion:** the output is valid bytes but displayed using the wrong character encoding.
- **Unauthenticated encryption:** modified ciphertext may not be detected.
- **Poor key management:** the key is exposed alongside the data it protects.

## Key takeaways

Cryptography is not one operation. Encoding changes representation, encryption protects confidentiality, hashing supports integrity checks, and signatures support authenticity and integrity. Use established algorithms, protect keys separately, and verify the entire implementation rather than judging security by the algorithm name alone.

## Expanded cryptography workflow

### 1. Start with the purpose of the operation

When a task presents a string, determine whether it is encoded, hashed, encrypted, or signed. These operations solve different problems. Encoding changes representation; hashing produces a digest; encryption uses a key to recover plaintext; and signatures help verify integrity and origin.

### 2. Classical ciphers

A Caesar cipher shifts letters by a fixed number. Atbash reverses the alphabet mapping. These are useful learning examples but are not secure encryption. For a Caesar cipher, try the shifts indicated by the room's clue and inspect whether the output forms meaningful text. Do not assume every unreadable string is a Caesar cipher.

### 3. Modern symmetric and asymmetric cryptography

Symmetric encryption uses a shared secret key. Asymmetric cryptography uses a public/private key pair. Public-key systems can support key agreement or signatures, but they do not automatically solve key distribution, identity validation, or secure storage.

### 4. Hashes and password storage

A hash is not normally decrypted. Password recovery tools compare candidate inputs against the stored hash. Password storage should use a purpose-built, salted password-hashing algorithm with appropriate work factors rather than a fast general-purpose hash.

### 5. Safe command-line inspection

For an encoded string, preserve the exact input and use a decoder suited to the format. For example, Base64 can be decoded with:

```bash
printf '%s' 'ENCODED_VALUE' | base64 --decode
```

For a hash, identify the algorithm and context instead of attempting to decode it. Never publish recovered real passwords or keys.

### 6. Common implementation failures

Weak randomness, reused keys, hard-coded secrets, missing authentication, and encryption without integrity protection can undermine otherwise strong algorithms. Use established libraries, authenticated encryption, secure key storage, and reviewed protocols. Avoid inventing custom cryptographic schemes.

### 7. Answering the room

Use the specific clues and values supplied by the active room. Explain the transformation and verify the result, especially if the output is another encoded string. Do not copy a demonstration ciphertext as a room answer; redact flags as `flag{hidden}` in public notes.
