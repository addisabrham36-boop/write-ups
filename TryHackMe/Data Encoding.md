# Data Encoding — TryHackMe

Room: https://tryhackme.com/room/dataencoding

Data encoding changes how information is represented so it can be stored or transmitted in a compatible format. It is different from encryption, because encoding does not require a secret key and is not intended to keep information confidential.

## Common representations

Binary represents values using bits. Hexadecimal uses sixteen symbols, from 0 to 9 and A to F, and is often used to display bytes compactly. Base64 represents binary data using a printable set of characters. URL encoding represents certain characters in a form that can safely appear in a URL.

On Linux, I can try Base64 with a harmless sample:

```bash
printf 'hello' | base64
printf 'aGVsbG8=' | base64 --decode
```

The first command encodes the text and the second decodes it. Base64 is reversible and provides no secrecy. If encoded data looks mysterious, I should first consider whether it is simply a representation change.

## Character encoding

Text also depends on a character encoding, such as UTF-8, that maps characters to bytes. If the same bytes are interpreted using a different encoding, the displayed text may look corrupted even though the underlying data has not changed.

## Working through a puzzle

I should inspect the characters and structure, look for familiar prefixes or patterns, and try plausible decoding methods one at a time. Repeatedly applying random encodings without recording each step makes the process confusing. If the output is binary or compressed data, a text decoder may not produce readable words.

## What I took from this room

Encoding is about representation, not secrecy. Base64, hex, URL encoding, and character encodings solve different practical problems. Recognising the format helps me interpret data correctly, while remembering that encoding is not encryption stops me from treating a reversible transformation as a security control.