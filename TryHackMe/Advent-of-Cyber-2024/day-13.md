# Advent of Cyber 2024 - Day 13: It came without buffering! It came without lag!

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What is the value of Flag1?

   We can see the tracking is currently tracking Glitch's car. Make sure to proxy the browser to Burpsuite.

   ![Glitch Car](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_13_Glitch_Car.png)

   Now make sure to untrack the car and turn off intercept. Now turn of intecept and click the track button.

   ![Intercept](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_13_Intercept.png)

   We can see the request made to track a certain car. 
   
   ![Avatar Glitch](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_13_Avatar_Glitch.png)
   
   If we change the user id to something else, we could track someone else's car. Changing this to '8', we can track Mayor Malwares car. From the message list on the right we can open the avatar for Mayor Malware in a new tab and see that his userid is in fact '8'.

   Forward the request and turn off intercept to start tracking. A flag will appear.

   ![Flag 1](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_13_Flag_1.png)

   ><details><summary>Click for answer</summary>THM{dude_where_is_my_car}</details>

2. What is the value of Flag2?

   To change the messages, lets see if we can intercept anything related to the messages. If we turn on intercept and keep forwarding the requests, eventually, a message request appears.

   ![Avatar Url](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_13_Avatar_Url.png)

   We can see the url for the avatar, same as what we used to find Mayor Malwares id. We also see a userid. We can try to change this to send messages as Mayor Malware with user id '8'. Make sure to have tracking turned on.

   Turn on intercept and type and send a message. In Burpsuite change the user id to 8 and forward the request (you can now turn of intercept).

   ![Send Message](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_13_Send_Message.png)

   ![Impersonate](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_13_Impersonate.png)

   ><details><summary>Click for answer</summary>THM{my_name_is_malware._mayor_malware}</details>

3. If you enjoyed this task, feel free to check out theBurp Suitemodule.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the value of Flag1? | `flag{hidden}` |
| What is the value of Flag2? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
