# Crack the Hash: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/crackthehash

## Room Overview & Information

**Category:** Password security / cryptography  
**Difficulty:** Easy  
**Focus:** Hash identification, wordlist attacks, Hashcat, John the Ripper, salts, and interpreting cracking results.

A hash is a one-way digest of input data. Password cracking does not decrypt the hash; it hashes candidate passwords and compares the results. Success depends on identifying the correct algorithm, understanding whether a salt is present, and using a candidate list that contains the original password.

Use the room's supplied hashes and wordlist. Do not attempt password attacks against accounts or systems outside an authorised lab.

## Phase 1: Identify the hash format

A hash's length and character set can suggest the algorithm, but they do not always uniquely identify it. For example, a 32-character hexadecimal value may be MD5 or another 128-bit format. Use multiple clues and confirm the candidate format before cracking.

Useful tools include:
- **HashID:** suggests likely hash types.
- **Hashcat:** performs GPU/CPU-assisted candidate testing.
- **John the Ripper:** supports many password hash formats.
- **CrackStation or similar lookup sites:** may identify common unsalted hashes, but should not be used for confidential data.

The room's hashes include MD5, SHA-1, SHA-256, bcrypt, MD4, NTLM, SHA-512 crypt, and HMAC-SHA1. These formats require different handling.

## Phase 2: Prepare the hashes

Create a text file with one hash per line, or keep a separate record of each hash and its likely algorithm. Do not combine unrelated hashes without tracking which result belongs to which question.

Calculate a file hash when you need to preserve an artefact's identity, but remember that password hashes are not all the same format. Salted formats include extra fields such as a salt or cost factor.

For this room, the main task is to identify each hash and test it against the supplied wordlist.

## Phase 3: Use Hashcat

Hashcat uses a mode number for each hash format. The mode must match the actual hash. Common modes relevant to the room include:

| Hash format | Hashcat mode |
|---|---:|
| MD5 | 0 |
| SHA-1 | 100 |
| SHA-256 | 1400 |
| bcrypt | 3200 |
| MD4 | 900 |
| NTLM | 1000 |
| SHA-512 crypt | 1800 |
| HMAC-SHA1, key is salt | 160 |

A basic wordlist attack has this form:

\`\`\`bash
hashcat -m MODE -a 0 HASH_FILE WORDLIST
\`\`\`

- **-m MODE** selects the hash algorithm.
- **-a 0** selects a straight/wordlist attack.
- **HASH_FILE** contains the hash or hashes.
- **WORDLIST** contains candidate passwords.

Use the mode that matches the hash format. If the room's AttackBox has the RockYou wordlist compressed, follow the room's instructions to make it available before running Hashcat.

## Task 1: Level 1

The first five hashes are cracked with the supplied wordlist. The results are:

| Hash format / clue | Recovered password |
|---|---|
| MD5 | `easy` |
| SHA-1 | `password123` |
| SHA-256 | `letmein` |
| bcrypt | `bleh` |
| MD4 | `Eternity22` |

The MD4 hash's password is not present in the standard RockYou list used by the example, so it may require a lookup source or another candidate list. The task is testing the recovered value, not merely whether one specific tool finds it.

## Task 2: Level 2

The remaining hashes include:

| Hash format / clue | Recovered password |
|---|---|
| SHA-256 | `paule` |
| NTLM | `n63umy8lkf4i` |
| SHA-512 crypt | `waka99` |
| HMAC-SHA1 with salt | `tryhackme` |

For the HMAC-SHA1 entry, the salt is `481616481616`. A salted hash must be represented in the format expected by the tool. If the salt is missing or placed incorrectly, the cracking result will not match.

## Step-by-step cracking workflow

1. Copy the hash exactly, including any prefix, salt, or cost parameters.
2. Identify likely formats using HashID and the room's hints.
3. Verify the candidate format against the tool's documentation.
4. Choose the matching Hashcat mode or John format.
5. Run a wordlist attack against the room-provided hash.
6. Inspect the result and map it back to the original hash.
7. Verify the recovered value against the task prompt.
8. Record which method worked and whether the password appeared in the wordlist.

Example for an MD5 hash:

\`\`\`bash
hashcat -m 0 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt
\`\`\`

Example for a SHA-1 hash:

\`\`\`bash
hashcat -m 100 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt
\`\`\`

Use a file containing only the relevant hash if you want to avoid mixing results from different formats. The exact path to RockYou may differ in the AttackBox.

## Troubleshooting

- **No password recovered:** the mode may be wrong, the wordlist may not contain the candidate, or the hash may need a salt.
- **Hashcat rejects the input:** inspect formatting, whitespace, line endings, and required salt fields.
- **Several algorithms appear possible:** use the task's context and test the likely candidates instead of trusting one detector.
- **A result appears but seems wrong:** check which hash it belongs to and confirm the output format.
- **A hash is slow to crack:** bcrypt and SHA-512 crypt deliberately require more work than fast hashes such as MD5.

## Security lessons

- Store passwords using a dedicated password-hashing function such as Argon2id, scrypt, or bcrypt with a unique salt.
- Do not use fast general-purpose hashes like MD5 or SHA-1 for password storage.
- Enforce long, unique passwords and screen against common or breached passwords.
- Use MFA where appropriate.
- Protect hash databases as sensitive data because they enable offline guessing.

## Key takeaways

Hash cracking is a process of classification and candidate testing. Identify the format, handle salts correctly, choose the right mode, and verify which candidate matches each hash. The room demonstrates why fast hashes and common passwords are weak choices for protecting user credentials.