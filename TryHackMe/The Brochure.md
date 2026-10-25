# The Brochure: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/hh-thebrochure-081f3e36

## Room Overview & Information

**Category:** OSINT and digital artefact analysis  
**Difficulty:** Beginner  
**Focus:** Image metadata, visible clues, public information, encoded fragments, and correlation.

The Brochure is part of the Hacker Holidays storyline. It begins with an image and asks the learner to follow clues toward a hidden connection. The challenge is to join several pieces of evidence without assuming that one metadata field or a matching name reveals the whole answer.

## Phase 1: Preserve the supplied artefact

Download the image from the active room and keep an unchanged copy. Record its exact filename, file size, and hash so later observations refer to the same artefact.

\`\`\`bash
file brochure-image.jpg
stat brochure-image.jpg
sha256sum brochure-image.jpg
exiftool brochure-image.jpg
\`\`\`

Replace the example filename with the actual file supplied by the room.

- **file** identifies the likely format from its contents.
- **stat** displays size and filesystem timestamps.
- **sha256sum** creates a digest for exact-file comparison.
- **exiftool** extracts available metadata.

If ExifTool is not installed, use the room's recommended alternative. Metadata may be absent or deliberately modified. A timestamp is not necessarily the true creation time, and an author field is not proof of identity.

## Phase 2: Inspect the visible image

Look closely at the brochure itself. Read text, logos, names, dates, QR codes, and unusual design elements. Zoom into small details rather than relying on a thumbnail. If the text is difficult to read, use OCR in the lab environment and compare the extracted text against the original image.

Keep three things separate in your notes:
1. What is visibly present in the image.
2. What metadata or a tool reports.
3. What you infer from those observations.

This separation prevents a plausible interpretation from being mistaken for direct evidence.

## Phase 3: Check for additional embedded data

If the room's clues suggest that the image may contain extra data, inspect the file structure without modifying the original:

\`\`\`bash
strings -n 5 brochure-image.jpg | head -n 80
xxd -l 128 brochure-image.jpg
\`\`\`

**strings** extracts printable sequences; **xxd** shows the first bytes in hexadecimal. These are triage tools. A lack of readable text does not prove that no embedded data exists, and a readable string may be unrelated to the challenge.

Use steganography tools only if the task provides a reason to suspect hidden content. Do not apply random tools until something appears to fit; first identify the clue and the file format.

## Phase 4: Follow the public trail

The room's storyline points toward an AI-related clue and a social-media trail. Record the exact names or phrases shown in the supplied artefact, then use them to locate the relevant public account, page, or post. Check that the account's content connects back to the brochure rather than relying on a name match alone.

For each relevant page:
- Record the URL and timestamp.
- Identify the phrase, image, or reference that links it to the prior clue.
- Check whether it points to another account or contains an encoded fragment.
- Preserve the original fragment before decoding it.
- Follow one link at a time so the chain can be reproduced.

Avoid collecting unrelated personal information. The objective is to solve the authorised puzzle, not to build a broader profile of a real person.

## Phase 5: Decode fragments carefully

If a public post contains an encoded string, inspect the character set and the room's hints before choosing a decoder. Base64 commonly uses letters, digits, plus and slash characters, with optional equals padding. Hexadecimal uses digits and letters A through F. Other ciphers may preserve letter patterns or use a clue from the story.

Keep the original text, the transformation used, and the resulting text in your notes. If the output is not meaningful, revisit the format instead of applying random transformations until something looks plausible.

Encoding is not encryption. A decoded result may contain another clue or another encoded layer; analyse each stage separately.

## Phase 6: Correlate clues

An OSINT conclusion should be supported by several consistent details. A matching name alone may be coincidence. A matching phrase, linked account, timestamp, and related artefact provide stronger support when they form a coherent chain.

Build a simple evidence table:

| Evidence | Direct observation | Interpretation | Confidence |
|---|---|---|---|
| Brochure image | Text, design, or metadata shown | Possible clue | Low until corroborated |
| Public post | Exact URL and content | Possible connection to the image | Medium if details match |
| Encoded fragment | Original string and decoded result | Follow-up clue | High only after verification |

The confidence rating is your assessment, not a substitute for evidence. Record contradictions rather than hiding them.

## Phase 7: Verify the final answer

Trace the final value to the last clue in the chain and check its exact spelling and formatting. Submit the value shown by the active room. Do not substitute a guessed flag or answer from a different instance. In a public note, represent a confirmed flag as `flag{hidden}`.

## Phase 8: Lessons and responsible use

- Preserve the original artefact and record its hash.
- Treat metadata as a clue, not proof.
- Verify public-account connections with more than one matching detail.
- Keep raw evidence separate from interpretation.
- Avoid collecting unrelated personal data.
- Document each transformation so another learner can reproduce the result.
- Use public information only within the room's intended purpose and scope.

## Key takeaways

OSINT combines small public clues into a supported conclusion. Image metadata, visible details, public accounts, and encoded text can each contribute, but every link in the chain needs evidence. A plausible story is not the same as a verified finding.