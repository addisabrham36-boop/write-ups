# Advent of Cyber 2023 - Day 9: She sells C# shells by the C2shore

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will be be investigating the malware sample we found in the previous challenge using dnsSpy.

1. What HTTP User-Agent was used by the malware for its connection requests to the C2 server?

   It seems all function we can find can be found in the main program section. Selecting this file, we can search it for any strings containing `agent`. This might give us the value of the useragent variable.

   ![User Agent](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D9_User_Agent.png)

   ><details><summary>Click for answer</summary>Mozilla/5.0 (Macintosh; Intel Mac OS X 14_0) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Safari/605.1.15</details>

2. What is the HTTP method used to submit the command execution output?

   Looking at the main program, we can see which function is called to submit the results from executed commands (shell and implant).

   ![Submit Function](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D9_Submit_Function.png)

   Looks like it is using `PostIt` to submit the data. We can look at this function to find the HTTP request method used.

   ![Submit Method](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D9_Submit_Method.png)

   ><details><summary>Click for answer</summary>POST</details>

3. What key is used by the malware to encrypt or decrypt the C2 data?

   We can find the this key by looking at the `decryptor` and `encryptor` function.

   ![Key](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D9_Key.png)

   ><details><summary>Click for answer</summary>youcanthackthissupersecurec2keys</details>

4. What is the first HTTP URL used by the malware?

   Firs this we should look at the main program file. Searching for `http` we can see where it is used first.

   ![Url](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D9_Url.png)

   Rember this is just the string containing part of the URL. The actual request (in GetIt) is don't with and additional argument.

   ><details><summary>Click for answer</summary>http://mcgreedysecretc2.thm/reg</details>

5. How many seconds is the hardcoded value used by the sleep function?

   The Sleeper function itself doesn't contain any hardcoded value, so we must look through the main program. By searching for usage of the `sleeper` function, we can see it uses the variable count.

   Searching for this variable gives us the harcoded value.

   ![Sleep Time](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D9_Sleep_Time.png)

   ><details><summary>Click for answer</summary>15</details>

6. What is the C2 command the attacker uses to execute commands via cmd.exe?

   Lets find out which function executes the `cmd` command on the machine. Looks like it is `ExecuteCommand`.

   ![Cmd Function](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D9_Cmd_Function.png)

   We can now search for where this function is called in the main program.

   ![Cmd Command](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D9_Cmd_Command.png)

   Looks like it is called in the IF statement which looks for a particular string.

   ><details><summary>Click for answer</summary>shell</details>

7. What is the domain used by the malware to download another binary?

   Lets look in the `implant` function to see what is happening there.

   ![Executable](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D9_Executable.png)

   Looks like a spyware program is downloaded to a particular folder. It doesn't show us the download domain though. Lets search for where this function is called in the main program.

   ![Dropper Url](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D9_Dropper_Url.png)

   Here we can see a URL being passed to the function containing the same spykit executable.

   ><details><summary>Click for answer</summary>stash.mcgreedy.thm</details>

Check out the [Malware Analysis](https://tryhackme.com/module/malware-analysis) module in the [SOC Level 2 Path](https://tryhackme.com/path-action/soclevel2/join) if you enjoyed analysing malware.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What HTTP User-Agent was used by the malware for its connection requests to the C2 server? | `Mozilla/5.0 (Macintosh; Intel Mac OS X 14_0) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Safari/605.1.15` |
| What is the HTTP method used to submit the command execution output? | `POST` |
| What key is used by the malware to encrypt or decrypt the C2 data? | `youcanthackthissupersecurec2keys` |
| What is the first HTTP URL used by the malware? | `http://mcgreedysecretc2.thm/reg` |
| How many seconds is the hardcoded value used by the sleep function? | `15` |
| What is the C2 command the attacker uses to execute commands via cmd.exe? | `shell` |
| What is the domain used by the malware to download another binary? | `stash.mcgreedy.thm` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
