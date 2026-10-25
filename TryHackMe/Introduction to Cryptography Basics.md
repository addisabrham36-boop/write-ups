# Introduction to Cryptography Basics: TryHackMe Walkthrough

Room: https://tryhackme.com/room/cryptographybasics

Cryptography protects information through techniques such as encryption, hashing, and digital signatures. These techniques solve different problems, so the first step in a question is to identify whether it is asking about confidentiality, integrity, authentication, or proof of origin.

## 1. Encoding, encryption, and hashing

- **Encoding** changes how information is represented so it can be transported or interpreted. It is not intended to keep information secret. Base64 is a common example.
- **Encryption** uses an algorithm and key to make data unreadable to someone who does not have the required key.
- **Hashing** maps input data to a fixed-size digest. A secure hash is designed to make it infeasible to recover the original input or find a different input with the same digest.
- **Digital signatures** use asymmetric cryptography to provide integrity and evidence that a signature was made with the corresponding private key.

A Base64 string is not encrypted just because it looks unreadable. Likewise, a hash is not a reversible encoding.

## 2. Symmetric encryption

Symmetric encryption uses the same secret key to encrypt and decrypt data. It is efficient for large amounts of data, but the parties need a secure way to share or establish the key.

AES is a widely used symmetric cipher. Its security depends on using it correctly, including an appropriate mode of operation, nonce or IV handling, and authentication where required. Choosing a strong cipher does not rescue a system that leaks the key.

## 3. Asymmetric cryptography

Asymmetric cryptography uses a key pair: a public key and a private key. The public key can be shared, while the private key must remain protected. Depending on the algorithm and protocol, asymmetric cryptography can support key establishment, encryption, or signatures.

A digital signature is created with the private signing key and verified with the corresponding public key. It does not automatically encrypt the signed message. Encryption and signing provide different properties.

## 4. Hashing and password storage

A cryptographic hash is useful for integrity checks and other security operations. Password storage needs a specialised password-hashing scheme such as Argon2id, scrypt, bcrypt, or PBKDF2, with a unique salt and appropriate cost settings. A fast general-purpose hash such as SHA-256 alone is not an ideal password-storage design because attackers can test guesses quickly.

A salt is not a secret key. It makes identical passwords produce different stored values and helps prevent precomputed attacks. Strong passwords and rate limiting still matter.

## 5. Public-key trust and certificates

A public key is useful only if the user can establish that it belongs to the intended party. Certificates bind identity information to a public key through a certificate authority and a chain of trust. Clients validate the certificate chain, hostname, validity period, and other requirements.

HTTPS uses TLS to establish a protected connection. A certificate warning may indicate an expired certificate, a hostname mismatch, an untrusted issuer, or another validation problem. It should not be ignored without understanding the cause.

## 6. Common mistakes

- Reusing a key across unrelated systems.
- Hardcoding keys or passwords in source code.
- Using obsolete algorithms or insecure modes.
- Reusing a nonce where the chosen algorithm requires uniqueness.
- Treating Base64 as encryption.
- Storing passwords with a fast unsalted hash.
- Assuming a digital signature also hides the message.
- Ignoring certificate validation failures.

## 7. Working through the room

When a question describes a security goal, match it to the correct primitive. Confidentiality generally points to encryption; integrity checking can use a hash or authenticated mechanism; proving that a message was signed by a key points to digital signatures; and converting data to a transport-friendly representation points to encoding.

Use the exact algorithm or terminology presented in the active room. If it asks you to decode a value, determine its format first and use a safe local tool. Never assume that every long hexadecimal string is a password hash or that every unreadable string is encrypted.

## What I took from this room

Cryptography is not one single operation. Encryption, hashing, encoding, and signing solve different problems, and security depends on key management and correct implementation as much as on the algorithm itself.