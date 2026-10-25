# The Brochure: Hacker Holidays 2026 — TryHackMe Walkthrough

Official room: https://tryhackme.com/room/hh-thebrochure-081f3e36

## Room Overview & Information

**Event:** Hacker Holidays 2026  
**Day:** 0, The Brochure  
**Category:** OSINT and image analysis  
**Difficulty:** Easy  
**Objective:** Inspect a hotel brochure, use its visible clues to find the correct social-media account, combine several fragments of an encoded message, and submit the flag.

This is a browser-based OSINT challenge, not a conventional port-scanning or boot-to-root machine. The intended clue is printed in the image and leads to a public social-media trail. The challenge does not require exploiting Instagram or accessing private accounts.

## Phase 1: Acquire and inspect the evidence

### 1. Download the room artifact

Open the Day 0 task and download the provided archive. Keep the original unchanged. On a Linux analysis system, inspect the archive before extracting it:

```bash
file task-files.zip
sha256sum task-files.zip
unzip -l task-files.zip
```

- `file` identifies the archive format.
- `sha256sum` records a hash so the source artifact can be identified later.
- `unzip -l` lists contents without writing extracted files to disk.

Use the actual filename if it differs from `task-files.zip`. When the archive contents look reasonable, extract it into a separate directory:

```bash
mkdir -p brochure-analysis
unzip task-files.zip -d brochure-analysis
find brochure-analysis -maxdepth 2 -type f -print
```

The expected artifact is a brochure image in PNG format. If the supplied archive differs, follow the actual filenames shown by `find`.

### 2. Inspect the image itself

Open the brochure in an image viewer and read the text carefully. The visible clues refer to finding the hotel on Instagram and to VERA, the hotel's AI concierge. The image is designed to look like an artifact-analysis puzzle, but the important clue is its visible content rather than a hidden executable or a network service.

You may inspect metadata as a sanity check:

```bash
file brochure.png
exiftool brochure.png
sha256sum brochure.png
```

These commands identify the file, show available image metadata, and record its hash. The lack of unusual metadata does not mean the image contains no clue; in this room, the intended pivot is printed on the brochure.

Do not spend time running exploit scanners or assuming that steganography is required. The task is explicitly categorized as OSINT.

## Phase 2: Follow the public social-media trail

### 3. Find the hotel account

Search the visible hotel name and Instagram reference. A useful query is:

```text
site:instagram.com "Byte Lotus Resort"
```

The query uses the search engine's `site:` operator to restrict results to Instagram and quoted words to narrow the hotel name. Open the matching public account and confirm that its displayed name and posts match the brochure. The account identified in the documented challenge is `thebytelotusresort`.

The profile has little useful information in its visible posts. Inspect the accounts it follows, because OSINT often relies on relationships between public accounts rather than one isolated page.

### 4. Pivot to VERA

The hotel account follows a single account that matches the brochure's VERA clue: `veratheconcierge`. Open that public profile and inspect its posts. Three posts contain separate fragments of a Base64-encoded message. Copy each fragment exactly and preserve the original order.

The three documented fragments are:

```text
VEhNe1YzckBzX2FD
QzB1bnRfaDRzX2Iz
M25fZjB1bmQhfQ==
```

Do not decode the pieces separately unless necessary. Base64 fragments may not end on a complete three-byte boundary; joining them first avoids corrupting the combined data.

## Phase 3: Decode and verify the message

### 5. Concatenate the fragments

Join the fragments with no spaces or line breaks:

```text
VEhNe1YzckBzX2FDQzB1bnRfaDRzX2IzM25fZjB1bmQhfQ==
```

Base64 is an encoding, not encryption. It changes the representation of bytes and does not provide confidentiality.

### 6. Decode using the command line

On Linux, copy the concatenated value into the following command:

```bash
printf '%s' 'VEhNe1YzckBzX2FDQzB1bnRfaDRzX2IzM25fZjB1bmQhfQ==' | base64 --decode
printf '\n'
```

- `printf '%s'` writes the exact input without appending a newline to the encoded data.
- `base64 --decode` converts the Base64 string back to its original text.
- The second `printf` adds a newline to make the terminal output easier to read.

Alternatively, paste the concatenated string into CyberChef and use the **From Base64** operation. Both methods should produce the same text.

### 7. Validate the flag

The decoded message is:

```text
THM{V3r@s_aCC0unt_h4s_b33n_f0und!}
```

Submit the value exactly, preserving capitalization, punctuation, and the exclamation mark.

## Phase 4: Question and answer

### Question: “What is the flag?”

**Answer:**

```text
THM{V3r@s_aCC0unt_h4s_b33n_f0und!}
```

**Evidence chain:** brochure text → public hotel Instagram account → account followed by the hotel → VERA's three encoded fragments → concatenated Base64 → decoded flag.

## Phase 5: Key Takeaways & Remediation

- OSINT findings often come from links between public profiles, not just from one page.
- Read the image before assuming the solution requires metadata, steganography, or exploit tooling.
- Keep public-source findings reproducible by recording the account name, post fragments, and decoding steps.
- Base64 is reversible representation, not encryption. Never treat an encoded secret as protected.
- Public accounts can expose unintended connections through their following lists and post history. Review what an organization and its service accounts reveal.
- Do not attempt to access private accounts, bypass platform controls, or collect unrelated personal data during OSINT.

This room is a reminder that the best first tool may be careful reading. The clue is visible, the trail is public, and the technical step is simply joining and decoding the three fragments.
