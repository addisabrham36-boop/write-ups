# Advent of Cyber 2024 - Day 23: You wanna know what happens to your hashes?

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. Crack the hash value stored in hash1.txt. What was the password?

   Using Hash ID we can identify the possible format of the hash.

   ```cmd
   python hash-id.py
   ```

   ![Format](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_23_Format.png)

   Looks like it is a SHA256 hash. We can crack this with `john` using rockyou as the wordlist and a set of rules.

   ```cmd
   john --format=raw-sha256 --rules=wordlist --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt
   ```

   ![Password](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_23_Password.png)

   ><details><summary>Click for answer</summary>fluffycat12</details>

2. What is the flag at the top of theprivate.pdf file?

   First we must fin the password for the pdf file. We can use `pdf2john` for this.

   We have done some investigating and found several possibilities for passwords and put them in a custom wordlist.
   
   ![Wordlist](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_23_Wordlist.png)

   ```cmd
   pdf2john.pl private.pdf > pdf.hash
   
   john --rules=single --wordlist=wordlist pdf.hash 
   ```

   ![Cracked](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_23_Cracked.png)

   We found the password. Now we can convert the file to a text file, to read the top of the file.

   ```cmd
   pdftotext private.pdf -uwp M4y0rMa1w4r3

   haed private.txt
   ```

   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_23_Flag.png)

   ><details><summary>Click for answer</summary>THM{do_not_GET_CAUGHT}</details>

3. To learn more about cryptography, we recommend theCryptographymodule. If you want to practice more hash cracking, please consider theJohn the Ripper: The Basicsroom.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Crack the hash value stored in hash1.txt. What was the password? | `fluffycat12` |
| What is the flag at the top of theprivate.pdf file? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
