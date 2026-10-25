# Data Encoding: TryHackMe Walkthrough

Room: https://tryhackme.com/room/dataencoding

Encoding changes the representation of data so it can be stored, transported, or interpreted by another system. It is not designed to keep the data secret. Security challenges often use encoding to make a value less obvious, so the first step is to recognise the format before trying to decode it.

## 1. Encoding versus encryption and hashing

- **Encoding:** reversible representation change, such as Base64 or URL encoding.
- **Encryption:** transforms data using a key to protect confidentiality.
- **Hashing:** produces a digest that is not intended to be reversed.
- **Compression:** reduces the size of data and may be reversible, but does not provide secrecy.

Base64 is not encryption. A hexadecimal string is not necessarily a hash. Identify the format from the context and structure rather than assuming every unreadable value is a cipher.

## 2. Common encodings

- **Base64:** uses letters, digits, plus, slash, and sometimes equals signs for padding.
- **Hexadecimal:** represents bytes using characters 0-9 and A-F.
- **URL encoding:** represents reserved or non-ASCII bytes using percent escapes.
- **Binary:** represents values as sequences of bits.
- **ASCII and UTF-8:** character encodings that map text to bytes.

The same text can be represented in several formats. For example, the word hello can be represented as ordinary text, Base64, hexadecimal bytes, or URL-encoded text. The encoded forms may look very different but decode to the same underlying data.

## 3. Decode one layer at a time

Start by copying the exact value from the room. Look at the character set, length, separators, and any padding. Use the tool or command appropriate to the suspected format, then inspect the output before trying another transformation.

On Linux, common tools include base64 for Base64, xxd for hexadecimal dumps, and Python's standard library for explicit conversions. For example, base64 -d decodes a Base64 input stream. Check the command's help if the installed implementation differs.

Do not repeatedly apply decoders at random. If the output is binary, determine whether it is a file or another encoded layer. If the output is gibberish, it may be compressed, encrypted, or simply decoded using the wrong format.

## 4. Recognise layered encoding

A challenge may apply multiple reversible transformations. After each step, record the input, the operation, and the output. This makes it easier to reverse the process and explain how the final value was obtained.

A decoded result may contain another encoding, but it may also contain ordinary text, a file signature, or encrypted content. Verify the next step from the new evidence.

## 5. Work with files safely

If the room provides a file rather than a string, use file to identify its type and calculate a hash before modifying it. Work on a copy and preserve the original. Do not execute unknown files just because their contents become readable after decoding.

## 6. Working through the room

For each question, identify the representation and use the appropriate decoder. Check the exact output and formatting. If the result is a flag or answer string, submit the value shown by the active room. Do not replace it with a guessed result from another guide.

## What I took from this room

Encoding is about representation, not secrecy. The most reliable approach is to identify the format, decode one layer at a time, inspect each result, and keep a record of the transformation.