# Crack the Hash: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/crackthehash

## Room Overview & Information

**Category:** Password security and cryptography  
**Difficulty:** Easy  
**Learning objectives:** Identify hash formats, understand salts, select Hashcat modes, run wordlist attacks, interpret results, and understand secure password storage.

This is a controlled password-recovery lab. Use only the provided hashes and wordlists. Do not attempt online password guessing against any system outside the room.

## Phase 1: Reconnaissance & Hash Identification

A hash is a fixed-length digest, not encrypted text. Password cracking tests candidate passwords against the hash. Length and character set can suggest an algorithm but do not uniquely identify every format; use task hints and a hash identification tool.

Inspect the input file and wordlist before starting:

```bash
file hashes.txt
wc -l hashes.txt
ls -lh /usr/share/wordlists/rockyou.txt
wc -l /usr/share/wordlists/rockyou.txt
```

The commands identify the file type, count input records, and check the wordlist's existence and size. In some AttackBox images, RockYou is compressed and needs to be prepared using the environment's instructions. The exact path may differ.

## Phase 2: Select the Cracking Method

### 1. Hashcat command structure

A generic wordlist attack is:

```bash
hashcat -m MODE -a 0 hashes.txt /path/to/wordlist.txt
```

- `-m MODE` selects the hash algorithm.
- `-a 0` selects a straight wordlist attack.
- `hashes.txt` contains hashes in the format expected by the chosen mode.
- The final argument is the candidate wordlist.

Common modes relevant to this room include:

| Format | Hashcat mode |
|---|---:|
| MD5 | 0 |
| SHA-1 | 100 |
| SHA-256 | 1400 |
| bcrypt | 3200 |
| MD4 | 900 |
| NTLM | 1000 |
| SHA-512 crypt | 1800 |
| HMAC-SHA1, key is salt | 160 |

Always confirm the exact format, because a salted format may include fields beyond the digest. Do not mix unrelated formats into one input file unless the tool format supports it.

### 2. Examples

For an unsalted MD5 hash:

```bash
hashcat -m 0 -a 0 md5.txt /path/to/rockyou.txt
```

For SHA-1:

```bash
hashcat -m 100 -a 0 sha1.txt /path/to/rockyou.txt
```

For HMAC-SHA1 with the room's salt, use the exact format expected by the selected mode. The salt listed in the room is `481616481616`. If the salt is missing or placed incorrectly, a valid password candidate will not match.

After the run, inspect the recovered results:

```bash
hashcat --show -m MODE hashes.txt
```

Replace MODE with the mode used on that file. The output should be mapped back to the original task's hash before submitting.

## Phase 3: Crack the Supplied Hashes

### Task 1: Level 1

The first group includes the following documented results:

| Hash clue | Recovered password |
|---|---|
| MD5 | `easy` |
| SHA-1 | `password123` |
| SHA-256 | `letmein` |
| bcrypt | `bleh` |
| MD4 | `Eternity22` |

The MD4 value may not appear in the standard RockYou list used by the example. If the wordlist attack fails, verify the hash mode and input formatting before trying another candidate source. The purpose is to understand that wordlists have limited coverage, not to assume a specific wordlist contains every password.

### Task 2: Level 2

The remaining documented results are:

| Hash clue | Recovered password |
|---|---|
| SHA-256 | `paule` |
| NTLM | `n63umy8lkf4i` |
| SHA-512 crypt | `waka99` |
| HMAC-SHA1 with salt `481616481616` | `tryhackme` |

Treat these values as reference answers for the published task version, not proof that your local execution succeeded. Keep each hash associated with its own algorithm and expected answer.

## Troubleshooting

- **No result:** the wordlist may not contain the password, or the algorithm mode may be wrong.
- **Invalid token or separator error:** verify spaces, newline characters, salt fields, and the exact input format.
- **Ambiguous format:** use the task context and tool documentation; hash length alone is not sufficient.
- **Recovered value appears unexpected:** ensure it corresponds to the right hash and challenge question.
- **Slow progress:** bcrypt and SHA-512 crypt are intentionally slower than MD5. That difference is part of why modern password-storage algorithms are chosen.

## Phase 4: Room Questions & Flags Walkthrough

For each hash, locate the corresponding task, identify the required algorithm, run the correct mode, and submit the candidate that matches. The room's expected recovery results are listed above. The exercise does not require network scanning or privilege escalation; its attack boundary is the offline cracking process applied to the supplied values.

Keep command output or a result note that links the input hash, mode, wordlist, and recovered candidate. Do not publish hashes from real users or repurpose recovered lab credentials.

## Phase 5: Key Takeaways & Remediation

MD5, MD4, NTLM, SHA-1, and fast SHA-256 are not suitable as direct password-storage schemes. Use Argon2id, scrypt, or bcrypt with unique salts and cost parameters appropriate to the deployment. Enforce long unique passwords, block common and breached passwords, apply MFA where appropriate, and protect password databases as sensitive material.

Hash cracking is a process of classification and candidate testing: identify the format, preserve salt details, choose a matching tool mode, and verify which candidate belongs to each task.