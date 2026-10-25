# Data Encoding: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/dataencoding

## Room Overview & Information

**Category:** Computer fundamentals  
**Difficulty:** Beginner  
**Focus:** ASCII, Unicode, UTF-16, UTF-32, and how computers represent text.

Computers store and process data as numbers and bytes. Character encodings define how characters map to numeric values. Encoding is not encryption: it is a representation that allows systems to store and exchange text consistently.

## Task 1: Introduction

The room introduces the idea that text is stored as numeric values. The correct interpretation depends on the character encoding being used.

## Task 2: ASCII

ASCII is a character encoding that maps letters, digits, punctuation, and control characters to numeric values. The classic ASCII set uses seven bits and defines values from 0 to 127. Many familiar characters have standard decimal codes.

### Question: “What is the ASCII code in decimal for the character @?”

**Answer:** **64**.

The at sign maps to decimal 64 in ASCII.

### Question: “What is the character that has the ASCII code of 35 in decimal?”

**Answer:** `#`.

Decimal 35 represents the number sign/hash character.

### Question: “What is the name of the character that has the ASCII code of 7?”

**Answer:** **BEL**.

BEL is the bell control character. It is a control code rather than a printable letter or symbol.

## Task 3: Unicode

Unicode assigns code points to characters across many writing systems and symbol sets. UTF-8, UTF-16, and UTF-32 are different ways of encoding Unicode code points as bytes or code units.

- **UTF-8** uses one to four bytes per code point.
- **UTF-16** uses one or two 16-bit code units.
- **UTF-32** uses a fixed 32-bit code unit for each code point.

A Unicode code point is commonly written as `U+` followed by hexadecimal digits. The number of hexadecimal digits in a displayed value does not necessarily indicate the number of bytes used by UTF-8.

### Question: “What is the UTF-32 encoding of the character U+1F60C (smiling face with closed eyes)?”

**Answer:** `U+0001F60C`.

UTF-32 displays the code point in a 32-bit form, padded with leading zeros.

### Question: “What is the UTF-16 encoding of シ? Note that ツ and シ are two different characters.”

**Answer:** `U+30B7`.

The character シ is Japanese katakana Shi. It is distinct from ツ, which is katakana Tsu.

### Question: “What is the character that has the following UTF-32 encoding U+0001F60E?”

**Answer:** The smiling face with sunglasses character, Unicode code point `U+1F60E`.

Use the code point to identify the character in a Unicode chart or the room's interactive viewer.

### Question: “What is the character that has the following UTF-16 encoding U+2658?”

**Answer:** The white chess knight, `U+2658`.

The character belongs to the Unicode chess-symbol block.

## How to verify a character

If you need to inspect a Unicode code point locally, Python can display its character and code point:

\`\`\`python
character = chr(0x1F60E)
print(character)
print(f"U+{ord(character):04X}")
\`\`\`

- `chr()` converts a numeric code point to a character.
- `ord()` returns the code point for a character.
- The format specifier `04X` prints hexadecimal with at least four digits.

For a UTF-16 or UTF-32 byte representation, the byte order and encoding form matter. A Unicode code point is not the same thing as the raw bytes stored by every encoding.

## Task 4: Conclusion

### Question: “If you are curious to see how computers manipulate data, join the Python Demo room (coming soon).”

**Answer:** No answer needed.

## Key takeaways

ASCII assigns numeric codes to a limited character set. Unicode defines code points for a much wider range of characters, while UTF-8, UTF-16, and UTF-32 encode those code points differently. Always distinguish a character's code point from the byte sequence used to store it.

## Expanded decoding workflow

### 1. Identify the representation before decoding

Encoding changes how data is represented; it does not necessarily conceal the content. Look at the character set, padding, separators, length, and task clues before choosing a decoder. Repeated blind decoding can turn a useful clue into an unstructured mess.

### 2. Base64

Base64 commonly uses letters, digits, plus, slash, and optional equals-sign padding. Decode a value supplied by the room with:

```bash
printf '%s' 'ENCODED_VALUE' | base64 --decode
```

Use the exact string from the task. If the command reports invalid input, check for copied whitespace, URL-safe Base64 characters, or missing padding. Do not assume the output is readable text; it may be binary or another encoded layer.

### 3. Hexadecimal

Hex represents each byte using two hexadecimal digits. Decode only when the string is valid hex:

```bash
printf '%s' '48656c6c6f' | xxd -r -p
```

The example decodes a demonstration string, not a room answer. Hex data can represent text, machine code, a hash, or arbitrary bytes, so interpret the result in context.

### 4. URL encoding

URL encoding represents bytes using percent escapes, such as `%20` for a space. It is common in query parameters and form submissions. Use the browser's URL tools or a trusted local decoder and preserve the original request for comparison. Do not decode twice unless the evidence shows the application performs multiple decoding passes.

### 5. Binary, octal, and character codes

Binary is a sequence of bits. If the task provides groups of eight bits, convert each group to a byte and then interpret it using the stated character encoding. Check whether the groups are separated consistently and whether leading zeros are significant. Octal and decimal character codes likewise require the correct character table.

ASCII covers a limited set of characters; Unicode uses broader code points and multiple encodings such as UTF-8. A number is not necessarily a character code just because it can be converted into a letter.

### 6. Layered encodings and ciphers

A string can be encoded multiple times or combined with a substitution cipher. Use structural clues to decide what to try next. Base64 often has recognizable padding and character distribution; hexadecimal contains only hex digits; Caesar or Atbash transformations preserve alphabetic structure. A sensible decoded result is a clue, not proof that the transformation was correct.

### 7. Troubleshooting

- Preserve the exact original input before changing it.
- Remove only accidental whitespace, not meaningful separators.
- Check whether the data is URL-safe Base64 or ordinary Base64.
- Use a byte-oriented tool if the result is not valid UTF-8.
- Do not decode hashes as if they were encrypted data.
- Verify the final result against the room's context and expected format.

### 8. Answer handling

For every question, write down the input, the selected encoding, the command or decoder used, and the resulting value. Retrieve the actual answer from the active room. Do not use a demonstration string as the room's answer, and redact any flag as `flag{hidden}` in public notes.
