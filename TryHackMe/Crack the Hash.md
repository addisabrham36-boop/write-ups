# Crack the Hash — TryHackMe

Room: https://tryhackme.com/room/crackthehash

This room introduces password hashes and the idea of recovering a password when a weak password has been hashed. A hash is not supposed to be decrypted like ciphertext. Instead, cracking tools make guesses, hash each guess using the expected method, and compare the result with the stored hash.

## Identify before cracking

The first step is to work out what kind of hash I have. Different algorithms produce different digest lengths, but length alone is not enough to identify every hash reliably. Context and the format matter too. If I choose the wrong mode in a cracking tool, even a correct password may not be found because the guesses are being processed incorrectly.

## Wordlist-based cracking

A common approach is to use a wordlist of likely passwords. In a controlled lab, tools such as John the Ripper or Hashcat can test those guesses. The exact command depends on the hash type, tool version, and the file format used by the room, so I should follow the room's instructions rather than paste a random command from another challenge.

A successful crack means a candidate matched the hash. It does not mean every password using that hash type is easy to recover. Weak, common passwords are much more likely to appear in a wordlist than long, random ones.

## Defending password storage

Applications should not store plaintext passwords. They should use a password-hashing scheme designed for password storage, such as Argon2id, scrypt, bcrypt, or PBKDF2 with appropriate settings, and a unique salt per password. Fast general-purpose hashes make large numbers of guesses much cheaper.

## What I took from this room

Hash cracking is mainly a guessing and verification process. Identifying the hash format, choosing the right mode, and using appropriate wordlists are important, but the deeper lesson is why password strength and safe password storage matter. I should record only the actual results I obtain in the lab and replace any recovered secret in a public write-up with `flag{hidden}` if it is a room flag.