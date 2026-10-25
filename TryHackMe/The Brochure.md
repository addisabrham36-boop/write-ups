# The Brochure: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/hh-thebrochure-081f3e36

## Room Overview & Information

**Event:** Hacker Holidays  
**Category:** OSINT  
**Difficulty:** Easy  
**Objective:** Analyse the supplied brochure, find the related public social-media account, inspect a suspicious follower, and recover the flag.

This is a short OSINT puzzle. The key is to follow the clues from the image to a public account and then inspect the unusual follower. The answer does not require attacking a server.

## Phase 1: Inspect the brochure

Download the brochure image from the active room and preserve the original. Check the file type and metadata:

\`\`\`bash
file brochure-image.jpg
exiftool brochure-image.jpg
sha256sum brochure-image.jpg
\`\`\`

Replace the example filename with the one supplied by the room. ExifTool may reveal useful metadata, but the challenge's intended path is based on the content and associated public information. The published walkthrough reports that running the image through Aperi'Solve does not reveal an obvious hidden payload, so do not assume every image puzzle requires steganography.

Read the text and branding in the brochure carefully. The important clue is the name **Byte Lotus Resorts**.

## Phase 2: Find the related Instagram account

Search Instagram for **Byte Lotus Resorts**. The clue leads to the public account:

\`\`\`text
@thebytelotusresort
\`\`\`

Open the account and inspect its visible profile information and followers. The room's intended clue is that the account has only one follower. That follower is suspicious in the context of the challenge and is the next lead.

Do not broaden the search into unrelated personal information. Follow only the account and profile clues required by the puzzle.

## Phase 3: Inspect the suspicious follower

Open the single follower's profile and inspect the visible username or encoded text. The room hints that the relevant value uses Base64. Base64 is an encoding, not encryption, and it can be decoded without a secret key.

If you have copied the encoded string into a file, decode it in the AttackBox:

\`\`\`bash
printf '%s' 'BASE64_VALUE' | base64 --decode
\`\`\`

Replace BASE64_VALUE with the exact string shown by the follower profile. The `printf` command avoids adding a newline to the input. Alternatively, use CyberChef's **From Base64** operation.

Keep the original string and decoded result in your notes. If the result is not readable, confirm that you copied the full value and that the encoding is actually Base64. Do not guess the flag from the account name.

## Phase 4: Recover and verify the flag

Follow the decoded clue and the profile information to identify the flag shown by the active challenge. Verify the exact characters and submit it to TryHackMe. This public write-up deliberately redacts the flag as `THM{hidden}`.

### Task question: “Uncover the flag!”

The complete evidence chain is:
1. Inspect the supplied brochure and identify the Byte Lotus Resorts clue.
2. Search Instagram for that name.
3. Open the account at `@thebytelotusresort`.
4. Inspect its single suspicious follower.
5. Decode the relevant Base64 value.
6. Use the result to recover the room's flag.

## Phase 5: Why this is OSINT

OSINT uses publicly available information to answer a specific question. The challenge does not depend on compromising Instagram or bypassing access controls. It relies on correlating a clue in one artefact with a public account and then interpreting the follower's encoded information.

A single match can be a coincidence, so connect each step to the preceding clue. In this room, the brochure branding, matching account, unusual follower count, and Base64 hint form the intended chain.

## Key takeaways

The Brochure is solved through clue correlation rather than exploitation. Read the image, search the exact brand name, inspect the suspicious follower, and decode the value indicated by the task. Preserve the original evidence and redact the flag in public notes.