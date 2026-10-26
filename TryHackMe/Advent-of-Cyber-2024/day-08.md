# Advent of Cyber 2024 - Day 8: Shellcodes of the world, unite!

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What is the flag value once Glitch gets reverse shell on the digital vault using port 4444? Note: The flag may take around a minute to appear in theC:\Users\glitch\Desktopdirectory. You can view the content of the flag by using the commandtype C:\Users\glitch\Desktop\flag.txt.

   I first tried using my own attackbox, but I couldn't reliably copy the script to the target machine. Through RDP there was not clipboard sharing. I could only use a tool to send clipboard as keystrokes. But this didn't paste everything correctly. And I would get many errors while executing. Running the entire script would also trigger MS Defender, so we indeed must paste it in parts.

   ![Errors](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_8_Errors.png)

   There seems to be an issue with copy pasting on this task. Nothing seems to work. In the end I tried transferring the script file from my attack box to the target machine by setting up an http server on port 80.

   ```cmd
   python3 -m http.server 80
   ```

   But I couldn't copy the entire script in one turn. No sir. MS Defender would flag and remove it. And since I don't have admin right, I can't restore it. So I had to send the script in three parts (just how we need to paste it into powershell). 

   ![Transfer](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_8_Transfer.png)

   Now we finally have our script on the target machine. Now we can simply paste each text file, because it is already split up.

   First set up a listener on our attack box.

   ```cmd
   nc -nlvp 1337 
   ```

   Nevermind, please don't make my mistake by using your elite port choice (1337) and use what is mentioned in the text (4444)...

   Copy paste the first shell part. Copy paste the second shell part and hit enter.

   Now copy paste the third part and hit enter. We should get a shell back.

   ![Reverse Shell](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_8_Reverse_Shell.png)

   Finally, we made it! Now we can look for the flag in the desktop folder.

   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_8_Flag.png)

   ><details><summary>Click for answer</summary>AOC{GOT _MY_ACCESS_B@CK007}</details>

2. Are you interested in learning more about evasion? Take a look at theAV Evasion: Shellcoderoom.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the flag value once Glitch gets reverse shell on the digital vault using port 4444? Note: The flag may take around a minute to appear in theC:\Users\glitch\Desktopdirectory. You can view the content of the flag by using the commandtype C:\Users\glitch\Desktop\flag.txt. | `AOC{GOT _MY_ACCESS_B@CK007}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
