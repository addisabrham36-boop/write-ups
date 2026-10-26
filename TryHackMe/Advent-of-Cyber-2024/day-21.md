# Advent of Cyber 2024 - Day 21: HELP ME...I'm REVERSE ENGINEERING!

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What is the function name that downloads and executes files in the WarevilleApp.exe?

   First we should open the binary in ILSpy. Then we look into the form for any function related to downloading.

   ![Function](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_21_Function.png)

   ><details><summary>Click for answer</summary>DownloadAndExecuteFile</details>

2. Once you execute the WarevilleApp.exe, it downloads another binary to the Downloads folder. What is the name of the binary?

   This can be found in the download function we found previously, or by running the executable and looking in the Downloads folder.

   ><details><summary>Click for answer</summary>explorer.exe</details>

3. What domain name is the one from where the file is downloaded after running WarevilleApp.exe?

   This can be found in the download function we found earlier.

   ><details><summary>Click for answer</summary>mayorc2.thm</details>

4. The stage 2 binary is executed automatically and creates a zip file comprising the victim's computer data; what is the name of the zip file?

   We can look through the computer until we find the zip file, or we can look through the explorer binary. Here we can see it creates a zip in the Pictures folder.

   ![Explorer](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_21_Explorer.png)

   ><details><summary>Click for answer</summary>CollectedFiles.zip</details>

5. What is the name of the C2 server where the stage 2 binary tries to upload files?

   Looking through the explorer binary, we see a function to upload files. This looks like a place where we can find the C2 server address.

   ![Upload](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_21_Upload.png)

   ><details><summary>Click for answer</summary>anonymousc2.thm</details>

6. If you enjoyed this task, feel free to check out thex86 Assembly Crash Courseroom.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the function name that downloads and executes files in the WarevilleApp.exe? | `DownloadAndExecuteFile` |
| Once you execute the WarevilleApp.exe, it downloads another binary to the Downloads folder. What is the name of the binary? | `explorer.exe` |
| What domain name is the one from where the file is downloaded after running WarevilleApp.exe? | `mayorc2.thm` |
| The stage 2 binary is executed automatically and creates a zip file comprising the victim's computer data; what is the name of the zip file? | `CollectedFiles.zip` |
| What is the name of the C2 server where the stage 2 binary tries to upload files? | `anonymousc2.thm` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
