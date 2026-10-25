# The Brochure — TryHackMe

Room: https://tryhackme.com/room/hh-thebrochure-081f3e36

The Brochure is an OSINT challenge from the Hacker Holidays storyline. The task starts with a brochure image and asks me to follow the clues to a hidden connection. The point is to combine image inspection and public information rather than assume the answer is visible in one obvious place.

## Inspect the provided image

I begin with the file supplied by the room and inspect its metadata and visible details. On Linux, ExifTool can display metadata when it is installed:

```bash
exiftool brochure-image.jpg
file brochure-image.jpg
```

I should replace the example filename with the actual downloaded file. Metadata can be missing or deliberately altered, so I should not rely on one field alone. I also inspect the image itself for visual clues and any text that may be embedded or encoded.

## Follow the public trail

The room describes a brochure image with an AI-related clue and a social-media trail. I should record the account names and clues exactly as shown by the provided artefacts, then follow the chain until it leads to the intended hidden connection. When a message or profile contains encoded fragments, I should decode them one step at a time and keep track of where each fragment came from.

I should avoid copying a full flag from another walkthrough. The evidence in the active room is what matters, and the public version of this write-up should replace the actual flag with `flag{hidden}`.

## What I took from this room

OSINT is about connecting small public clues into a supported conclusion. Image metadata, visual details, social accounts, and encoded text can each contribute, but I should verify the links between them instead of assuming that a coincidence proves the answer.