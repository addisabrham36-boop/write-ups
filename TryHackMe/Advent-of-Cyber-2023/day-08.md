# Advent of Cyber 2023 - Day 8: Disk Forensics

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Dead-box disk forensics using **AccessData FTK Imager** to recover deleted files, inspect master file tables, and calculate cryptographic disk hashes.

## Walkthrough
1. Mount the forensic image `usb_drive.e01` into FTK Imager.
2. Verify image integrity hash: The SHA-1 hash is computed as `39f2dea6ffb43bf80d80f19d122076b3682773c2`.
3. Expand deleted directories (marked with red cross icons):
   - In folder `DO NOT OPEN`, inspect deleted text notes: C2 domain is revealed as `mcgreedysecretc2.thm`.
   - Deleted archive contains `JuicyTomaTOY.exe`.
   - Viewing deleted image `portrait.png` in the FTK hex viewer reveals plaintext strings: `THM{byt3-L3vel_@n4Lys15}`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the malware C2 server? | `mcgreedysecretc2.thm` |
| What is the file inside the deleted zip archive? | `JuicyTomaTOY.exe` |
| Flag hidden in one of the deleted PNG files? | `THM{byt3-L3vel_@n4Lys15}` |
| SHA1 hash of the forensic image? | `39f2dea6ffb43bf80d80f19d122076b3682773c2` |
