# Psycho Break: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/psychobreak

## Room Overview & Information

**Category:** Linux boot-to-root / themed CTF  
**Difficulty:** Medium  
**Themes:** Nmap enumeration, web-source inspection, Atbash cipher, OSINT image research, directory discovery, restricted command execution, nested archives, Morse audio, multi-tap keypad encoding, password recovery, and privilege escalation.

Psycho Break is inspired by *The Evil Within*. The machine presents a chain of clues: each room on the map provides information needed to unlock the next one. Start the machine, record its assigned IP, and use only the active TryHackMe instance. Room keys, target addresses, user/root flags, and some results may vary; this write-up gives the reasoning and exact transformation where it can be verified, while keeping unverified values out of the public copy.

## Phase 1: Reconnaissance & Web Enumeration

### Task 1: Recon

Start the target and save a full service scan:

```bash
nmap -sC -sV -O -oN psychobreak-nmap.txt TARGET_IP
```

- `-sC` runs Nmap's default scripts.
- `-sV` attempts to identify service versions.
- `-O` enables operating-system detection.
- `-oN` saves normal output to a file.

Replace TARGET_IP with the room's current address. The intended machine exposes FTP, SSH, and a web service, with Ubuntu as the likely OS. Confirm the actual ports and results in your current scan rather than copying a port table from another instance.

Keep the scan as evidence. An open service is an attack surface, not proof that the service is vulnerable.

### Task 2: Web

Open the target's web service and inspect the homepage source using View Page Source or:

```bash
curl -i http://TARGET_IP/
```

The page source contains a comment pointing to:

```text
/sadistRoom
```

Visit the path and inspect the page, links, and any messages. A route hidden in a source comment is only an information-disclosure clue; it is not access control.

### Obtain the locker-room key

The Sadist room contains an interaction that reveals the locker-room key. Read the value from your active page and submit it in the room. Dismiss the pop-up as instructed and inspect the updated page.

Do not confuse the locker-room key with the later map key or Keeper key. They unlock different steps.

### Decode the map message with Atbash

The locker-room page presents this string:

```text
Tizmg_nv_zxxvhh_gl_gsv_nzk_kovzhv
```

This is Atbash, a substitution cipher that maps the alphabet in reverse: A ↔ Z, B ↔ Y, and so on. Applying the mapping gives:

```text
Grant me access to the map please
```

Submit the phrase as the task requests, then follow the map links to Safe Heaven and the Abandoned Room. This is a simple substitution cipher, not modern encryption.

### Find the Keeper key in Safe Heaven

Open Safe Heaven and inspect its page source for additional comments and clues. The room indicates that directory discovery is needed. Run Gobuster against the named path only:

```bash
gobuster dir -u http://TARGET_IP/SafeHeaven/ -w /path/to/wordlist.txt -o safeheaven-enum.txt
```

- `dir` selects directory/file discovery.
- `-u` supplies the base URL.
- `-w` selects a wordlist available in the AttackBox.
- `-o` saves the results.

Inspect discovered paths manually; the relevant path leads to the Keeper page.

The Keeper page has a timed image-location challenge. Use reverse-image search on the supplied image and corroborate the match using distinctive visible landmarks. Submit the location required by the room and copy the Keeper key shown by the current instance. Do not guess the location from the theme or reuse a value from an old run.

## Phase 2: Vulnerability Analysis & Initial Access

### Task 3: Help Mee

### Enter the Abandoned Room

Use the Keeper key to access the Abandoned Room through the map. The page includes an escape interaction and a source comment mentioning a shell-related parameter. Inspect both the HTML source and the HTTP request before testing. The lesson is that the application restricts commands using a weak input-handling design.

Test only the smallest non-destructive command that the room permits. Record the exact request parameter and response. Do not treat a rendered page or an error message as proof that a command executed.

### Discover the hidden directory and archive

The room's hint points toward listing a parent directory. Use the page's intended test to identify the hidden directory, then access only the challenge artifacts required to continue. Save the archive into the lab workspace and inspect its real file type:

```bash
file helpme.zip
unzip -l helpme.zip
```

- `file` identifies the likely format from file contents.
- `unzip -l` lists archive members without extracting them.

Do not assume the file extension proves the format, and do not execute unknown files from the archive.

### Extract the nested archive

The archive includes a text note and a file named `Table.jpg`. Check its type:

```bash
file Table.jpg
```

The file named Table.jpg is actually another ZIP archive. Extract it into a separate directory and list the contents:

```bash
mkdir -p /tmp/psychobreak-work
unzip Table.jpg -d /tmp/psychobreak-work
find /tmp/psychobreak-work -maxdepth 3 -type f -print
file /tmp/psychobreak-work/*
```

The nested archive contains an image and a WAV file. Read the accompanying note for the clue about the person locked in the cell, then investigate the audio artifact.

### Decode the WAV clue

Identify the audio format and duration:

```bash
file clue.wav
ffprobe -hide_banner clue.wav
```

Replace `clue.wav` with the actual filename extracted from the archive. Use the Morse audio decoder or waveform/spectrogram viewer instructed by the room. Preserve the spacing between Morse characters and words; spacing errors can change the decoded result. Verify the output against the clue and submit the word displayed by the challenge.

Do not invent a decoded phrase if the audio is unclear. Return to the exact supplied file and check that you extracted the correct archive member.

### Recover the FTP credentials

Use the credentials revealed by the earlier clues to authenticate to the FTP service found during reconnaissance. List the available files and download only the relevant challenge artifacts. Check each downloaded file before opening it:

```bash
file program
file random.dic
strings -n 5 program | head -n 40
```

The compiled program and dictionary are part of the challenge. Treat them as untrusted files and run them only in the lab workspace.

## Phase 3: Password Recovery and Privilege Escalation

### Task 4: Crack it open

The supplied program accepts one candidate word and reports whether it is correct. Running the program without the required argument may reveal its usage. The dictionary file contains the candidate list.

Use a controlled loop to test each word against the supplied program. The exact executable syntax should be taken from its usage output. Do not substitute a general password cracker or apply the wordlist to unrelated services.

The accepted candidate makes the program print a sequence of long numbers. These values are not ordinary decimal numbers; the room indicates they represent multi-tap input from an old mobile-phone keypad.

### Decode the multi-tap message

In multi-tap encoding, repeated presses on one digit correspond to different letters. For example, on a traditional keypad the 2 key maps to A, B, and C depending on the number of presses. Separate letter groups are usually represented by a different delimiter or timing boundary.

Group the numeric output according to the task's format, translate each group using the correct keypad mapping, and verify that the decoded phrase makes sense in context. Submit the phrase required by the room. Do not invent the program's accepted password or numeric output; read those values from the active binary's output.

### Task 5: Go Capture The Flag

Use the access path established by the previous stages to obtain a shell. Check the current context:

```bash
whoami
id
hostname
pwd
ls -la
sudo -l
```

These commands verify the account and host, list the current directory, and show permitted sudo commands. Continue the room's intended local privilege checks only after confirming the initial account. Verify the effective identity after every privilege transition; a user shell is not root access.

Retrieve the user flag from the path indicated by the active room. Follow the room's final privilege-escalation clue to reach the root flag, then verify root-level context before reading it. Public notes should redact both flags as `flag{hidden}`.

### Bonus: Defeat Ruvik

The bonus task mentions the clue `the_eye_of_ruvik` and asks the learner to remove the Ruvik account. This is a privileged action. Confirm the user and associated home directory, then follow the official room's requested procedure only on the lab target. Verify the account state after the action. Do not remove accounts on any system outside the CTF.

## Phase 4: Room Questions & Evidence Checklist

| Task | Evidence to preserve |
|---|---|
| Recon | Nmap output showing live ports, service versions, and OS estimate |
| Web clue | Homepage source comment and Sadist-room route |
| Locker-room key | Exact value shown in the current room |
| Map phrase | Atbash ciphertext and decoded plaintext |
| Keeper key | Image-location source and key shown after the timed task |
| Hidden directory | Relevant request and directory listing |
| Archive chain | File types, archive listings, and nested artifact names |
| Audio clue | WAV file and verified Morse decoding |
| FTP access | Login success and downloaded challenge filenames |
| Password program | Accepted candidate and program output |
| Multi-tap clue | Numeric groups and decoded text |
| User/root flags | Exact values from the current host, redacted publicly |
| Bonus task | Account state before and after the authorized action |

The exact locker-room key, Keeper key, image location, password candidate, numeric sequence, and flag values depend on the active environment and must be copied from the current instance. This guide does not invent them.

## Phase 5: Key Takeaways & Remediation

- Remove sensitive clues and backups from public web roots.
- Do not expose command-execution functionality through weakly filtered request parameters.
- Validate file types based on content and store uploads outside executable paths.
- Restrict FTP and other legacy services to authorised users and protect credentials.
- Avoid weak passwords and protect password-checking utilities.
- Patch vulnerable components, restrict privileged files and commands, and audit account permissions.
- Verify each step with evidence before using its output as the next assumption.

Psycho Break is a chain of different techniques. The reliable way through it is to verify every clue, use the right transformation, preserve the original artifact, and confirm each access or privilege transition instead of guessing.
