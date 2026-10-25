# Cryptography for Dummies — TryHackMe

Room: https://tryhackme.com/room/cryptographyfordummies

Cryptography is about protecting information and making sure it can be trusted. It is easy to mix up encoding, encryption, and hashing because all three can make text look different. They do different jobs, though, and mixing them up can lead to some very bad security decisions.

## Encoding is not encryption

Encoding changes how data is represented so that another system can handle or transmit it. Base64 is a common example. I can encode a small test string from the terminal:

```bash
printf 'hello' | base64
```

I can decode Base64 with:

```bash
printf 'aGVsbG8=' | base64 --decode
```

This is not secret protection. Anyone who knows the encoding can reverse it, so Base64 should never be used to hide a password or confidential message.

## Encryption protects confidentiality

Encryption transforms readable data into ciphertext using an algorithm and a key. Someone who does not have the right key should not be able to recover the original information in a practical amount of time, assuming the system is designed and used correctly.

**Symmetric encryption** uses the same secret key to encrypt and decrypt data. It is efficient for large amounts of data, but the key must be shared securely.

**Asymmetric cryptography** uses a related public and private key pair. The public key can be shared, while the private key must be protected. This supports tasks such as key exchange and digital signatures, depending on the algorithm and protocol.

A strong algorithm alone is not enough. Poor key management, weak passwords, insecure modes, or implementation mistakes can still expose the data.

## Hashing is different again

A cryptographic hash function takes input and produces a fixed-length digest. It is designed to be one-way, so the digest is not meant to be decrypted back into the original message. Even a small change to the input should produce a very different digest.

Hashing is useful for integrity checks and other security tasks. Password storage needs special care: systems should use password-hashing algorithms designed to be slow and resistant to guessing, with a unique salt for each password. A fast general-purpose hash by itself is not the right way to store user passwords.

## Digital signatures and trust

Digital signatures use asymmetric cryptography to help verify who signed data and whether it changed after signing. In a simplified view, the sender signs with a private key and others verify the signature using the matching public key. A valid signature does not automatically mean the content is safe or truthful; it helps establish integrity and authenticity under the relevant trust model.

## What I took from this room

The important distinction is:

- **Encoding** changes representation and is usually reversible without a secret.
- **Encryption** protects confidentiality and requires the appropriate key to decrypt.
- **Hashing** produces a digest that is not designed to be reversed.
- **Digital signatures** help verify integrity and authenticity.

These concepts appear all over cybersecurity, from password storage and secure web traffic to file verification and digital certificates. When I see an unfamiliar string, I should not assume it is encrypted just because it looks unreadable. First I need to work out what kind of transformation produced it.
