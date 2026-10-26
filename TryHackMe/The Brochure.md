# The Brochure

Official room: https://tryhackme.com/room/hh-thebrochure-081f3e36

## Room Overview & Information

**Event:** Hacker Holidays  
**Category:** Open-source intelligence (OSINT)  
**Difficulty:** Easy  
**Objective:** Analyse the supplied brochure, find the related public social-media account, inspect its unusual follower, and recover the room flag.

This is an OSINT puzzle, not a server-exploitation challenge. The intended path is to correlate a clue in the brochure with a public Instagram account, then decode information revealed by the suspicious follower. No network scanning, shell access, or privilege escalation is required.

## Phase 1: Acquire and Inspect the Brochure

### 1. Download and preserve the image

Open the active room and download the provided brochure image. Keep the original file unchanged. Confirm the filename and format:

```bash
file brochure-image.jpg
sha256sum brochure-image.jpg
exiftool brochure-image.jpg
```

- `file` identifies the likely format from file contents.
- `sha256sum` records a fingerprint of the exact artifact.
- `exiftool` displays available image metadata.

Replace the example filename with the one supplied by the room. Metadata may contain useful clues, but the original write-up reports that Aperi'Solve does not reveal an obvious hidden payload. Do not assume every image puzzle requires steganography.

### 2. Read the visible clue

Inspect the brochure's text, branding, and visual details. The key name in the artifact is **Byte Lotus Resorts**.

The goal at this stage is not to guess the flag. Identify a distinct phrase from the brochure and use it to search public information. Preserve the exact spelling because a small variation can make the correct profile harder to find.

## Phase 2: Follow the Public Instagram Trail

### 3. Find the resort account

Search Instagram for **Byte Lotus Resorts**. The matching public profile identified by the room is:

```text
@thebytelotusresort
```

Open the profile and inspect the visible posts, profile details, and follower list. The challenge's next clue is that this account has only one follower.

Do not broaden the search into unrelated personal information or attempt to access private accounts. The intended chain needs only the public account and its suspicious follower.

### 4. Inspect the single follower

Open the follower's public profile and carefully read the visible username or encoded text. The room indicates that the relevant value is Base64-encoded.

Base64 changes how bytes are represented as text. It is not encryption and requires no secret key to reverse. The presence of Base64 does not by itself make a string malicious; the room's clue and the profile context tell you what to inspect next.

## Phase 3: Decode and Validate the Clue

### 5. Copy the encoded value exactly

Copy the complete Base64 string from the follower profile. Preserve the original string before decoding it. Missing a character, including spaces incorrectly, or accidentally truncating the text can produce a decoding error or a different result.

### 6. Decode using the AttackBox

Use the following command, replacing BASE64_VALUE with the exact string from the active profile:

```bash
printf '%s' 'BASE64_VALUE' | base64 --decode
printf '\n'
```

- `printf '%s'` writes the string without appending a newline to the encoded input.
- `base64 --decode` decodes the text.
- The final `printf` prints a newline so the terminal output is easier to read.

Alternatively, paste the value into CyberChef and use the **From Base64** operation.

If decoding fails, verify that the full string was copied and that the task's value is actually Base64. Do not switch to a different cipher simply because the first attempt did not work.

### 7. Follow the decoded clue

Read the decoded output and follow the exact clue it provides. The decoded value leads to the flag in the active challenge; use the current room's output as the authority.

This note deliberately does not supply a guessed encoded string or flag value. The exact follower content is the evidence that connects the social-media clue to the answer, and it must be read from the live room.

## Phase 4: Task Question and Evidence Chain

### Question: “Uncover the flag!”

Use the sequence below to complete the task:

1. Inspect the supplied brochure and identify the Byte Lotus Resorts clue.
2. Search Instagram for the exact name.
3. Open the matching account, `@thebytelotusresort`.
4. Inspect the single follower identified by the room.
5. Copy the encoded value shown by that profile.
6. Decode the value with Base64.
7. Follow the resulting clue and submit the flag shown by the active room.

The flag should be redacted in this public repository as `THM{hidden}`, but the live task requires the exact value shown by TryHackMe.
