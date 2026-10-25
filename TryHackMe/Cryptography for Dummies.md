# Cryptography for Dummies: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/cryptographyfordummies

## Room Overview & Information

**Category:** Cryptography fundamentals  
**Difficulty:** Beginner  
**Focus:** Classical ciphers, modern cryptographic concepts, keys, hashes, and recognising common transformations.

Cryptography is the study and practice of protecting information. In a CTF, a string that looks strange may be encoded or encrypted, and the challenge is to identify the transformation. In real systems, cryptography protects data, but it depends on correct key management and implementation.

## Phase 1: Identify the problem before decoding

Start with the task's clues. Check the character set, string length, punctuation, and whether the task mentions a cipher, key, or encoding. Preserve the original value so you can compare every transformation.

Do not blindly run a long list of decoders. Choose a plausible method, test it, and evaluate whether the result is meaningful in the context of the task.

## Phase 2: Caesar and substitution ciphers

A Caesar cipher shifts letters by a fixed number of positions. For example, shifting each letter by three maps A to D, B to E, and so on. Because there are only 26 possible letter shifts in the basic English alphabet, it is easy to brute-force as a puzzle.

Atbash maps the alphabet in reverse: A to Z, B to Y, C to X, and so on. It uses no secret key.

Classical substitution ciphers are useful learning exercises but are not secure for modern communication. Frequency analysis and the small keyspace make them vulnerable.

## Phase 3: Base64 and hexadecimal

Base64 and hexadecimal are encodings, not encryption.

```bash
printf '%s' 'hello' | base64
printf '%s' 'aGVsbG8=' | base64 --decode
printf '%s' 'hello' | xxd -p
```

The first command encodes a string, the second decodes a Base64 value, and the third displays the bytes of a string in hexadecimal. Use `printf` to avoid adding an unwanted newline.

If the decoded bytes are not readable text, inspect the output as a file or hex dump. The data may be binary or may contain another layer of encoding.

## Phase 4: Hashes

A hash maps data to a fixed-length digest. It is not designed to be decrypted. If a CTF provides a hash and asks for the original input, the intended approach may be to identify the hash type and compare candidate values against the digest.

A hash lookup or password-cracking tool can only find a candidate that matches; it does not reverse the hash mathematically. The result depends on the candidate set and the strength of the original input.

For files, compute a digest with:

```bash
sha256sum sample.bin
```

Use the hash to compare exact file identity, not to determine whether the file is malicious by itself.

## Phase 5: Encryption and keys

Encryption uses a key to transform plaintext into ciphertext. Symmetric encryption uses a shared secret, while asymmetric cryptography uses a related public/private key pair. The correct key and parameters are needed for decryption.

If a challenge mentions a key, identify where it is provided and how it is expected to be used. Do not confuse a password, key, IV/nonce, salt, and hash; they have different purposes. A salt is not necessarily secret, while an encryption key must be protected.

## Phase 6: A repeatable CTF method

1. Copy the exact input into a note.
2. Identify likely encodings from their character sets.
3. Check the task description for a cipher or algorithm clue.
4. Apply one transformation at a time.
5. Save each intermediate result.
6. Verify that the output makes sense and matches the task's expected format.
7. If it fails, revisit the hypothesis instead of stacking random transformations.
8. Submit the value returned by the active challenge and redact flags in public notes.

## Phase 7: Security lessons

Do not invent custom encryption. Use established cryptographic libraries, authenticated encryption, strong key derivation, and a secure process for storing and rotating keys. Never put credentials or secret keys in public source repositories.

## Key takeaways

The first skill is classification: encoding, encryption, hashing, and classical ciphers are different. Read the clue, choose a justified transformation, preserve intermediate values, and verify the result. Cryptography is easier to learn when each operation has a clear purpose.