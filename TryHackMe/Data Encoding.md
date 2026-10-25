# Data Encoding: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/dataencoding

## Room Overview & Information

**Category:** Cryptography fundamentals  
**Difficulty:** Beginner  
**Focus:** Recognising common data representations, decoding safely, and distinguishing encoding from encryption and hashing.

Encoding changes how data is represented so it can be stored, transmitted, or interpreted by another system. It is not necessarily a security mechanism. Base64, hexadecimal, URL encoding, and character encodings are common examples. A string that looks random may be encoded, encrypted, compressed, or simply binary data rendered as text; appearance alone is not enough to identify it.

## Phase 1: Identify the representation

Start by checking the context. Ask where the value came from, which characters it contains, whether its length fits a common format, and whether the room gives a clue about the transformation.

Useful local tools include:

```bash
file sample.txt
xxd -g 1 sample.txt | head
strings -n 4 sample.bin | head
```

- `file` guesses a file type from its content.
- `xxd -g 1` displays bytes in hexadecimal groups.
- `strings` extracts readable sequences from a binary file.

These tools provide clues, not certainty. A file extension or a readable substring does not prove the complete format.

## Phase 2: Base64

Base64 represents binary data using a set of 64 printable characters, commonly including letters, digits, `+`, `/`, and optional `=` padding. It is useful for transport, not secrecy.

Decode a Base64 string from a file:

```bash
base64 --decode encoded.txt
```

Encode a short text:

```bash
printf '%s' 'hello' | base64
```

Use `printf` rather than `echo` when you want to avoid an added newline changing the encoded bytes. On systems where the command option differs, check `base64 --help`.

A valid-looking Base64 string can decode to binary rather than readable text. If the output is unreadable, inspect it with `file` or `xxd` instead of assuming the decoding failed.

## Phase 3: Hexadecimal

Hexadecimal represents each byte using two hexadecimal digits. It is common in packet captures, file dumps, hashes, and binary analysis.

Convert a file to a hex view:

```bash
xxd sample.bin | head
```

A byte such as decimal 65 is hexadecimal `41`, which corresponds to ASCII `A`. Hex is a representation of bytes, not encryption. A hex string can represent text, machine code, compressed data, or arbitrary binary content.

## Phase 4: URL encoding

URLs have reserved characters that may need percent-encoding. For example, a space is often represented as `%20` in a URL. URL encoding is not a way to protect secrets; the server can decode it.

When examining a suspicious URL, distinguish the actual path and query parameters from their encoded representation. Repeated or nested encoding can make a value harder to read, but it does not automatically make it safe or malicious.

## Phase 5: Character encodings

ASCII maps a limited set of characters to numeric values. UTF-8 is a variable-length encoding for Unicode and is widely used on the web. A byte sequence interpreted with the wrong character encoding may display incorrectly even when the underlying bytes are intact.

If text appears corrupted, preserve the original bytes and identify the expected encoding before converting it. Avoid repeatedly converting a file in place because this can destroy the original evidence.

## Phase 6: Encoding versus encryption versus hashing

- **Encoding:** changes representation and is generally reversible without a secret.
- **Encryption:** protects confidentiality and requires a key to decrypt.
- **Hashing:** produces a fixed-length digest designed not to be reversed.
- **Compression:** reduces data size and may also be reversible, but serves a different purpose.

A Base64 string is not encrypted merely because it is unreadable at first glance. Likewise, a SHA-256 digest cannot be decoded to recover the original file.

## Phase 7: Working through the room

For each challenge, preserve the original string and identify the likely format before applying a decoder. Use one transformation at a time and inspect the output. If a decoded result appears to contain another encoding, record the first result and then analyse the next layer separately.

When a question asks for a specific decoded value, use the exact value supplied by the active room. Do not copy an answer from a different challenge instance or invent a flag.

## Key takeaways

Encoding is about representation, not confidentiality. Identify the likely format from context, use the appropriate tool, preserve the original bytes, and verify the output before interpreting it. This discipline is useful in CTF puzzles, malware triage, web traffic analysis, and digital forensics.