# Advent of Cyber 2023 - Day 8: Have a Holly, Jolly Byte!

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will be using FTK Imager to examine a malicious USB drive and recover any deleted items.

1. What is the malware C2 server?

   Lets examine some of the files on the disk. The deleted 'DO NOT READ` folder seems promising. Here we have a secret text file that might be of interest.

   Opening it, we can see it is some sort of chat log containing information about the C2 server.
   
   ![C2 Server](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D8_C2_Server.png)

   ><details><summary>Click for answer</summary>mcgreedysecretc2.thm</details>

2. What is the file inside the deleted zip archive?

   We can see the deleted zip file. We can click on it to reveal its contents. Looks like there is a malicious executable within.
   
   ![File](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D8_File.png)

   ><details><summary>Click for answer</summary>JuicytomaTOY.exe</details>

3. What flag is hidden in one of the deleted PNG files?

   Looking at both images in the root folder, there is nothing in the image that resemles a flag.

   However, one of the images seems to be somewhat corrupted. Perhaps someone messed with the bytes of the file. 

   ![Image](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D8_Image.png)

   We can switch to using the hex-view mode to look at the bytes inside the image file. Using the search function we can look for `THM{`.

   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D8_Flag.png)

   ><details><summary>Click for answer</summary>THM{byt3-L3vel_@n4Lys15}</details>

4. What is the SHA1 hash of the physical drive and forensic image?

   The has can be found by selecting the image in the file tree window and verifying the disk. This gives us another windows with various hashes.
   
   ![Hash](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D8_Hash.png)

   ><details><summary>Click for answer</summary>39f2dea6ffb43bf80d80f19d122076b3682773c2</details>

If you liked today's challenge, the [Digital Forensics Case B4DM755](https://tryhackme.com/room/caseb4dm755) room is an excellent overview of the entire digital forensics and incident response (DFIR) process!

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the malware C2 server? | `mcgreedysecretc2.thm` |
| What is the file inside the deleted zip archive? | `JuicytomaTOY.exe` |
| What flag is hidden in one of the deleted PNG files? | `flag{hidden}` |
| What is the SHA1 hash of the physical drive and forensic image? | `39f2dea6ffb43bf80d80f19d122076b3682773c2` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
