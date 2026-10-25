# Data Encoding: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/dataencoding

## Room Overview & Information

**Category:** Computer fundamentals  
**Difficulty:** Beginner  
**Topics:** ASCII, Unicode, UTF-8, UTF-16, UTF-32, hexadecimal, Base64, and how computers represent text.

This is an educational room rather than a penetration-testing target. Network scanning, initial access, and privilege escalation are not applicable. The practical work is to identify the requested encoding and interpret the exact code point or output.

## Phase 1: Reconnaissance & Enumeration of the Input

Before converting a value, identify whether the task asks for a character code, Unicode code point, sequence of encoded bytes, or an encoding such as Base64. These are not interchangeable:

- **Character:** the visible symbol or text.
- **Code point:** the number assigned to a character by Unicode.
- **Encoding:** a rule for representing code points as bytes or code units.
- **Base64:** a reversible representation of bytes using printable characters.
- **Hash:** a digest intended not to be reversed.
- **Encryption:** a keyed operation intended to provide confidentiality.

A code point does not uniquely describe the bytes of every encoding form. The same Unicode point can be represented as different byte sequences in UTF-8 and UTF-16.

## Phase 2: Character Encoding Tasks

### Task 1: Introduction

The room introduces the idea that text is stored and processed as numeric values. Character encodings define how text maps to numbers and bytes.

### Task 2: ASCII

ASCII is a seven-bit character encoding covering values 0 through 127. It includes letters, digits, punctuation, and control characters.

**Question:** “What is the ASCII code in decimal for the character @?”

**Answer:** **64**.

Verify locally:

```python
print(ord("@"))
```

The ord() function returns the numeric code point for the character. For ASCII characters, that value matches the classic ASCII code.

**Question:** “What is the character that has the ASCII code of 35 in decimal?”

**Answer:** **#**.

Verify:

```python
print(chr(35))
```

The chr() function converts a valid Unicode code point to its character.

**Question:** “What is the name of the character that has the ASCII code of 7?”

**Answer:** **BEL**.

BEL is the bell control character. It is not a printable alphabetic character; historically, it signals an alert on compatible terminals or devices.

### Task 3: Unicode

Unicode assigns code points to characters across a wide range of scripts and symbols. UTF-8, UTF-16, and UTF-32 are different ways of encoding Unicode code points.

- **UTF-8:** one to four bytes per Unicode code point.
- **UTF-16:** one or two 16-bit code units.
- **UTF-32:** one 32-bit code unit for each code point.

A Unicode code point is written as U+ followed by hexadecimal digits.

**Question:** “What is the UTF-32 encoding of the character U+1F60C (smiling face with closed eyes)?”

**Answer:** **U+0001F60C**.

UTF-32 represents the code point in a 32-bit form, padded with leading zeros.

**Question:** “What is the UTF-16 encoding of シ? Note that ツ and シ are two different characters.”

**Answer:** **U+30B7**.

The character シ is katakana Shi. It differs from ツ, katakana Tsu.

**Question:** “What is the character that has the following UTF-32 encoding U+0001F60E?”

**Answer:** Smiling face with sunglasses, code point **U+1F60E**.

**Question:** “What is the character that has the following UTF-16 encoding U+2658?”

**Answer:** White chess knight, **U+2658**.

### Verify Unicode locally

Python can display a character from a code point and show its numeric value:

```python
character = chr(0x1F60E)
print(character)
print(f"U+{ord(character):04X}")
```

- chr() converts a numeric Unicode code point into a character.
- ord() returns the code point for a character.
- The 04X format prints uppercase hexadecimal with at least four digits.

If a question requests raw UTF-16 or UTF-32 bytes, byte order and encoding form matter. Do not answer with the code point alone unless the question asks for the code point or uses the room's notation.

### Task 4: Conclusion

**Question:** “If you are curious to see how computers manipulate data, join the Python Demo room (coming soon).”

**Answer:** No answer needed.

## Phase 3: Troubleshooting and Common Misinterpretations

- Do not confuse decimal with hexadecimal.
- Do not confuse a Unicode code point with an encoded byte sequence.
- A visible symbol may consist of more than one code point, for example with combining marks or emoji sequences.
- UTF-8 and UTF-16 can encode the same character differently.
- ASCII control codes such as BEL may not display as printable glyphs.
- Base64 is not encryption and does not require a secret key.

## Phase 4: Room Questions & Answer Summary

| Question | Answer |
|---|---|
| Decimal ASCII code for @ | **64** |
| Character at ASCII decimal 35 | **#** |
| Name of ASCII code 7 | **BEL** |
| UTF-32 encoding of U+1F60C | **U+0001F60C** |
| UTF-16 encoding of シ | **U+30B7** |
| Character for U+0001F60E | Smiling face with sunglasses |
| Character for U+2658 | White chess knight |
| Join the Python Demo room | No answer needed |

## Phase 5: Key Takeaways & Remediation

Character encodings let software consistently represent text across systems. Use the exact representation requested by the task, and distinguish the character, its Unicode code point, and its encoded bytes. When debugging corrupted text, check the source encoding, byte order where relevant, and whether the data was decoded more than once. Do not use encoding as a substitute for encryption or access control.