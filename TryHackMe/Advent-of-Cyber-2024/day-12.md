# Advent of Cyber 2024 - Day 12: If I can’t steal their money, I’ll steal their joy!

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What is the flag value after transferring over $2000 from Glitch's account?

   First step is to open up our browser and Burpsuit. Then we can enable out Burpsuite proxy (Foxyproxy) to intercept the traffic in Burpsuite.

   After login into the dashboard, we can see the requests in the history list.

   ![Login](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_12_Login.png)

   We can see that manually trying to transfer $2000,- doesn't work as it detect that we have insufficient funds. However, we can use race conditions with Burpsuite.

   ![Insufficient](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_12_Insufficient.png)

   In Burpsuite, we can use the previously created transfer request and modify it after sending it to Repeater.

   ![Repeater](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_12_Repeater.png)

   Duplicate these windows a couple of times using Crtl + R and group them together.

   ![Group](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_12_Group.png)

   Now we can run all requests in parallel. In our browser session we can see more than our balance has been transferred.

   ![Send](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_12_Send.png)

   However, I did this with the tester account and should have done it with the glitch account. But the steps remain the same. Hold on while I do this again with the correct account.

   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_12_Flag.png)

   ><details><summary>Click for answer</summary>THM{WON_THE_RACE_007}</details>

2. If you enjoyed this task, feel free to check out theRace Conditionsroom!

3. Where balances shift and numbers soar, look for an entry - an open door!

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the flag value after transferring over $2000 from Glitch's account? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
