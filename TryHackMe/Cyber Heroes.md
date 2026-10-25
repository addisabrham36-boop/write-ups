# Cyber Heroes — TryHackMe

Room: https://tryhackme.com/room/cyberheroes

I need to verify the exact room tasks before writing a detailed solution, because themed rooms can include several unrelated mini-challenges. I should not invent puzzle answers or flags based on the title alone.

## My approach

I start by reading the scenario and identifying the type of evidence in each task. If it provides a file, I inspect its type and contents. If it provides a web page, I examine the visible clues and the relevant request or response. If it asks for encoding or decoding, I identify the likely format and test it on the supplied data.

For file-based clues, these basic commands can help:

```bash
file sample
sha256sum sample
strings sample | head -n 40
```

They identify the file type, calculate a hash, and display some printable strings. They are generic inspection commands, not claimed outputs from this room.

## What I took from this room

The reliable way to solve a multi-part challenge is to treat each clue as evidence, use the right tool for the data type, and verify the result before moving on. I should add the exact answers and redacted flag locations only after confirming them in the active room.