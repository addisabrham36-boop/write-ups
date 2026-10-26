# Advent of Cyber 2024 - Day 6: If I can't find a nice malware to use, I'm not going.

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What is the flag displayed in the popup window after the EDR detects the malware?

   First we must start the EDR. We can do this with the following command:

   ```powershell
   .\JingleBells.ps1
   ```

   No events have been found yet. So now we can run the malware now.

   After executing the malware, a pop-up window appears with the flag. The event is also displayed in the powershell terminal.

   ![Flag 1](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_6_Flag_1.png)

   ><details><summary>Click for answer</summary>THM{GlitchWasHere}</details>

2. What is the flag found in the malstrings.txt document after running floss.exe, and opening the file in a text editor?

   To analyze the file we use Floss with the following command:

   ```powershell
   .\FLOSS\floss.exe .\Malware\MerryChristmas.exe | Out-File malwarestrings.txt
   ```

   Now we can open the resulting text file and look for the flag.

   ![Flag 2](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_6_Flag_2.png)

   ><details><summary>Click for answer</summary>THM{HiddenClue}</details>

3. If you want to more about sandboxes, have a look at the roomFlareVM: Arsenal of Tools.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the flag displayed in the popup window after the EDR detects the malware? | `flag{hidden}` |
| What is the flag found in the malstrings.txt document after running floss.exe, and opening the file in a text editor? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
