# Advent of Cyber 2024 - Day 1: Maybe SOC-mas music, he thought, doesn't come from a store?

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. Looks like the song.mp3 file is not what we expected! Run "exiftool song.mp3" in your terminal to find out the author of the song. Who is the author?Correct Answer

   We can download any youtube video using its link on the website linked on the system. Either select mp3 or mp4.

   ![Link](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_1_Link.png)

   ![Download](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_1_Download.png)

   Using `file` we can check out the files. We can see the second file `somg.mp3` is not what we would expect.

   ![Files](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_1_Files.png)

   Using Exiftool we can find out the author of the song.

   ```cmd
   exiftool song.mp3
   ```

   ![Artist](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_1_Artist.png)

   ><details><summary>Click for answer</summary>Tyler Ramsbey</details>

2. The malicious PowerShell script sends stolen info to a C2 server. What is the URL of this C2 server?Correct AnswerHint

   Using exiftool on the other file we see there is a link to a powershell file on a Github page.

   ![Github Link](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_1_Github_Link.png)

   Navigating to this link we see the malicious script. At the bottom we can find the address of the c2 server.

   ![Script](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_1_Script.png)

   ><details><summary>Click for answer</summary>http://papash3ll.thm/data</details>

3. Who is M.M? Maybe his Github profile page would provide clues?Correct Answer

   To find out more about M.M. we can go to the Github profile we can derive from the powershell script url. On this page we can open the M.M. repo.

   ![Github Repo](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_1_Github_Repo.png)

   ![Github Profile](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_1_Github_Profile.png)

   Seems we have a name here!

   ><details><summary>Click for answer</summary>Mayor Malware</details>

4. What is the number of commits on the GitHub repo where the issue was raised?

   Searching for issues on Github containing "Created by the one and only M.M." we can find another interesting clue.

   ISSUES

   ![Github Commits](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_1_Github_Commits.png)

   ><details><summary>Click for answer</summary>6791</details>

5. If you enjoyed this task, feel free to check out theOPSECroom!

6. What's with all these GitHub repos? Could they hide something else?

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Looks like the song.mp3 file is not what we expected! Run "exiftool song.mp3" in your terminal to find out the author of the song. Who is the author? | `Tyler Ramsbey` |
| The malicious PowerShell script sends stolen info to a C2 server. What is the URL of this C2 server? | `http://papash3ll.thm/data` |
| Who is M.M? Maybe his Github profile page would provide clues? | `Mayor Malware` |
| What is the number of commits on the GitHub repo where the issue was raised? | `6791` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
