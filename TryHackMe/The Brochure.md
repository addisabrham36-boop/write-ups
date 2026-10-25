# The Brochure: TryHackMe Walkthrough

Room: https://tryhackme.com/room/hh-thebrochure-081f3e36

The Brochure is an OSINT challenge from the Hacker Holidays storyline. It begins with a brochure image and asks the learner to follow public clues toward a hidden connection. The task is about joining several pieces of evidence, not assuming that one metadata field reveals the whole answer.

## 1. Download and preserve the provided image

Use the file supplied by the active room and keep an unchanged copy. Record the filename and hash so that later observations refer to the same artefact.

On Linux, inspect its type and metadata:

    file brochure-image.jpg
    sha256sum brochure-image.jpg
    exiftool brochure-image.jpg

Replace brochure-image.jpg with the actual filename. If ExifTool is unavailable, follow the room's recommended tool or inspect the image through another safe local utility.

Metadata can reveal a timestamp, software name, author field, or other clue, but metadata may be absent or deliberately modified. A timestamp is not necessarily the time the image was created, and an author field is not proof of identity.

## 2. Inspect the visible content

Look closely at the brochure itself. Read text, logos, names, dates, QR codes, and unusual design elements. If the image includes text that is hard to read, zoom in or use OCR in the lab environment. Keep the extracted text separate from your interpretation.

The room's storyline mentions an AI-related clue and a social-media trail. Record the exact terms that appear in the supplied image and use them to guide the next search. Do not replace them with similar names from an unrelated write-up.

## 3. Follow the public trail

Use the clue to locate the relevant public account, page, or post. Check whether the profile and content actually connect to the original artefact. Record the URL and the reason it appears relevant. If a post contains encoded fragments or references another account, follow each link one step at a time.

When decoding a fragment, identify the likely representation first. Keep the original string, the transformation used, and the resulting text. If the result is not meaningful, revisit the format instead of applying random transformations until something looks plausible.

## 4. Correlate clues rather than relying on coincidence

An OSINT conclusion should be supported by multiple consistent details. A matching name alone may be coincidence. A matching phrase, linked account, timestamp, and related artefact provide stronger support when they form a coherent chain.

Avoid collecting unrelated personal information. The objective is to solve the authorised puzzle, not to build a broader profile of a real person.

## 5. Verify the final answer

Trace the final value to the last clue in the chain and check its exact spelling and formatting. Submit the value shown by the active room. This public note does not include an unverified flag; after confirming it in the lab, replace it with flag{hidden} if a redacted flag placeholder is needed.

## What I took from this room

OSINT combines small public clues into a supported conclusion. Image metadata, visible details, public accounts, and encoded text can each contribute, but every link in the chain needs evidence. A plausible story is not the same as a verified finding.