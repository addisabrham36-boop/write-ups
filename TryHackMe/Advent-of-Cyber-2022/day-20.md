# Advent of Cyber 2022 - Day 20: Firmware Analysis

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
IoT firmware extraction, file system carving with **Binwalk**, and cryptographic key recovery.

## Walkthrough
1. Extract file structures from firmware image:
   ```bash
   binwalk -e firmwarev2.2-encrypted.gpg
   ```
2. The firmware contains an encrypted GPG blob and a script containing a hardcoded recovery key.
3. Decrypt the firmware payload with GPG:
   ```bash
   gpg --decrypt firmwarev2.2-encrypted.gpg
   ```
4. Reading the decompressed firmware source code exposes the flag: `THM{WE_GOT_THE_FIRMWARE_CODE}`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Flag value after reversing `firmwarev2.2-encrypted.gpg`? | `THM{WE_GOT_THE_FIRMWARE_CODE}` |
