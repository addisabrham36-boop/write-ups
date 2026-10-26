# Advent of Cyber 2023 - Day 18: A Gift That Keeps on Giving

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will be looking at rogue services and how they can prevent you from stopping a malicious process.

1. What is the name of the service that respawns the process after killing it?

   Checking the system resources with `top` we can see a process that is using up 100% of the CPU.

   ![Top](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D18_Top.png)

   We can try killing it using `sudo kill 651`, but it simply respawn again.

   ![Kill](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D18_Kill.png)

   We checked the crontabs for our user and root, but got no results...

   ```cmd
   crontabs -l
   sudo -i
   crontabs -l
   ```

   Next thing to do is check the processes that er running with `systemctl list-unit-files`.

   ![Processes](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D18_Processes.png)

   Looks like there is a process that might be malicious. Lets look closer to see if it really is malicious.

   ```cmd
   systemctl status a-unkillable.service
   ```

   ![Process](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D18_Process.png)

   ><details><summary>Click for answer</summary>a-unkillable.service</details>

2. What is the path from where the process and service were running?

   In the image above we can see from where the service is loaded and where the files are stored on the machine.

   ><details><summary>Click for answer</summary></details>

3. The malware prints a taunting message. When is the message shown? Choose from the options below.

   1. Randomly
   
   2. After a set interval
   
   3. On process termination
   
   4. None of the above

   We can see from the process information below when the message is displayed.

   ![Taunt](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D18_Taunt.png)

   After the exe has been (re)started the message is displayed. So we can rule out options 1 and 2.

   Everytime the process is killed, the service restarts it and the message is displayed again. However, it is not displayed on termination of the process but when it is first started.

   ><details><summary>Click for answer</summary>4</details>
   
If you enjoyed this task, feel free to check out the [Linux Forensics](https://tryhackme.com/room/linuxforensics) room.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the name of the service that respawns the process after killing it? | `a-unkillable.service` |
| The malware prints a taunting message. When is the message shown? Choose from the options below. | `4` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
