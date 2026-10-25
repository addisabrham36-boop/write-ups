# Crack the Hash — TryHackMe

Room: https://tryhackme.com/room/crackthehash

This room is about identifying hash types and recovering weak passwords from hashes using wordlists and cracking tools. The important part is not just running John the Ripper. I need to identify the likely hash format first, choose the correct mode, run the attack, and check the recovered result against the task.

I only use these techniques on the hashes provided by this room or on data I am authorised to test. A hash is not the same thing as encryption: a password hash is designed to be one-way, but weak passwords can still be guessed and compared against it.

## 1. Prepare the lab

1. Open the room and read the task's instructions.
2. Start the AttackBox if I am using it.
3. Copy the exact hash from the question into a text file. Do not add extra spaces or punctuation.
4. Keep the original value in a separate file so I can verify that I did not accidentally change it.

For a simple one-hash task:

```bash
printf '%s\n' 'PASTE_HASH_HERE' > hash.txt
cat hash.txt
```

The string `PASTE_HASH_HERE` is a placeholder, not a real challenge hash.

## 2. Identify the hash type

Hash strings can look similar, so I should not choose an algorithm only because the string is hexadecimal. Check its length and structure, then use a hash-identification tool if available:

```bash
hashid 'PASTE_HASH_HERE'
```

HashID reports likely algorithms. It does not prove which algorithm was used, because different algorithms can produce strings with similar appearance. The room's clues and known format help narrow it down.

Common examples include MD5 (often 32 hexadecimal characters) and SHA-1 (often 40 hexadecimal characters), but length alone is not conclusive. Password-storage formats may also include a salt, algorithm marker, and cost parameter.

## 3. Check which tools are installed

John the Ripper is available in many security training environments:

```bash
john --version
john --list=formats | head
```

If the room provides a wordlist such as RockYou, check that the file exists before launching the attack:

```bash
ls -lh /usr/share/wordlists/
```

On some systems, the RockYou file may be compressed. I should use the actual path provided by the lab and follow its instructions for extracting it if needed.

## 4. Run a wordlist attack

For a hash format that John supports, the general pattern is:

```bash
john --format=FORMAT --wordlist=/path/to/wordlist.txt hash.txt
```

Replace `FORMAT` with the format appropriate for the supplied hash and replace the wordlist path with the real file in the lab. Do not copy the placeholder literally.

John hashes each candidate password using the selected algorithm and compares the result with the target hash. If the format is wrong, the input file is malformed, or the password is not in the wordlist, the attack may not recover anything.

For a format John can identify automatically, I can first try:

```bash
john --wordlist=/path/to/wordlist.txt hash.txt
```

The explicit `--format` option is useful when automatic detection is ambiguous.

## 5. Display recovered passwords

After John finishes, show the recovered results:

```bash
john --show hash.txt
```

If it finds a password, the output associates the recovered plaintext with the corresponding hash entry. I should submit only the value requested by the current task.

If nothing is recovered:

1. Check that the hash was copied exactly.
2. Confirm the selected format is correct.
3. Confirm that the wordlist path exists and is readable.
4. Check whether John reports that the hash is already cracked.
5. Re-read the room's hint. Some hashes need a different wordlist or attack method.

Do not conclude that a hash is strong simply because one wordlist did not crack it. That result only means the tested candidates did not match.

## 6. Understand what cracking proves

A recovered password demonstrates that the password was present in the tested candidate set and the hash format was correct. It does not mean every hash using that algorithm is recoverable. Strong, unique passwords and modern password hashing with salts and a deliberately expensive work factor make offline guessing much harder.

MD5 and SHA-1 are not suitable choices for new password storage. Modern systems should use a dedicated password hashing algorithm such as Argon2id, scrypt, bcrypt, or PBKDF2 with appropriate parameters.

## 7. Record each room answer

For each hash in the room, I should make a small table in my notes with:

- The task number or label.
- The observed hash format and why I chose it.
- The tool and command used.
- Whether the wordlist attack succeeded.
- The recovered answer, kept private in this public repository as `flag{hidden}` when it is a flag.

I should not paste made-up output into the write-up. The exact hash values and passwords depend on the room's task content, so I record them from the active room rather than invent them here.

## What I took from this room

Hash cracking is a process: preserve the input, identify the format, choose the right tool, test a sensible wordlist, and verify the result. The algorithm matters, but password quality and storage configuration matter just as much. A cracking tool does not magically decrypt a hash; it tests candidate passwords until one matches.

Reference: https://tryhackme.com/room/crackthehash
