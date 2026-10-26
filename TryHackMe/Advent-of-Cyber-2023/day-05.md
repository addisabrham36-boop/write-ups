# Advent of Cyber 2023 - Day 5: A Christmas DOScovery: Tapes of Yule-tide Past

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will look at file signatures and how we can use them to using MsDOS.

1. How large (in bytes) is the AC2023.BAK file?

   After opening the DosBox executable we are greeting with the welcome screen.

   ![DosBox](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D5_DosBox.png)

   We can now view the size of the backup file by using `dir`.

   ![Size](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D5_Size.png)

   ><details><summary>Click for answer</summary>12,704</details>

3. What is the name of the backup program?

   In the backup folder we can find the Bumaster program, this name alone is not sufficient. So we can read the readme file to see if there is another name inside.

   ![Name](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D5_Name.png)

   ><details><summary>Click for answer</summary>BackupMaster3000</details>

5. What should the correct bytes be in the backup's file signature to restore the backup properly?

   If we try to restore the file using the Bumaster program we get an error message about the file signature.

   ```cmd
   bumaster.exe C:\ac2023.bak
   ```

   ![Error](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D5_Error.png)

   It mentions to read the readme file. Reading further into this file, we can see a section called troubleshooting which tells us which bytes the beginning of the file must contain.

   ![Troubleshooting](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D5_Troubleshooting.png)

   ><details><summary>Click for answer</summary>41 43</details>

7. What is the flag after restoring the backup successfully?

   Using Cyberchef we can find out which characters we need to put at the beginning of the file.

   ![Signature](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D5_Signature.png)

   Opening the backup file, we can indeed see that the two bytes at the beginning of the file are wrong (XX).

   ![File](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D5_File.png)

   Replacing 'XX' with 'AC' and re-running the command, we have successfully restored the backup.

   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D5_Flag.png)

   ><details><summary>Click for answer</summary>THM{0LD_5CH00L_C00L_d00D}</details>

What you've done is a simple form of reverse engineering, but the topic has more than just this. If you are interested in learning more, we recommend checking out our [x64 Assembly Crash Course room](https://tryhackme.com/room/x86assemblycrashcourse), which offers a comprehensive guide to reverse engineering at the lowest level.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| How large (in bytes) is the AC2023.BAK file? | `12,704` |
| What is the name of the backup program? | `BackupMaster3000` |
| What should the correct bytes be in the backup's file signature to restore the backup properly? | `41 43` |
| What is the flag after restoring the backup successfully? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
