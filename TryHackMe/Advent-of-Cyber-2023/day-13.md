# Advent of Cyber 2023 - Day 13: To the Pots, Through the Walls

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will be looking at the Diamond Model and how to use firewalls and honeypots to harden our security posture.

1. Which security model is being used to analyse the breach and defence strategies?

   This model is repeatetly mentioned in the text.

   ><details><summary>Click for answer</summary>Diamond Model</details>

2. Which defence capability is used to actively search for signs of malicious activity?

   The answer is given in the text.

   ><details><summary>Click for answer</summary>Threat Hunting</details>

3. What are our main two infrastructure focuses? (Answer format: answer1 and answer2)

   This answer to, can be found in the text. These are tools we will be using.

   ><details><summary>Click for answer</summary>Firewall and Honeypot</details>

4. Which firewall command is used to block traffic?

   While editing the policies for the firewall we come accross two terms that determine what happens to a connection.

   ![Firewall Rules](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D13_Firewall_Rules.png)

   ><details><summary>Click for answer</summary>Deny</details>

5. There is a flag in one of the stories. Can you find it?

   In our home folder we have a firewall rule script. Lets run it to properly setup the firewall.

   ```cmd
   sudo bash Van_Twinkle_rules.sh
   ```

   ![Firewall Active](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D13_Firewall_Active.png)

   Lets check the script and see what rules have been added.

   ![Http Server](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D13_Http_Server.png)

   Two rules seem to be of interest to us. Running a quick nmap we can confirm we indeed are looking for the webserver on port 8090.

   ![Nmap](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D13_Nmap.png)

   Lets allow connections to this port in our firewall.

   ![Http Allow](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D13_Http_Allow.png)

   Now we should be able to access the website.

   ![Website](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D13_Website.png)

   Now we must investigate the website and look for our flag.

   ><details><summary>Click for answer</summary>THM{P0T$_W@11S_4_S@N7@}</details>

If you enjoyed this task, feel free to check out the [Network Device Hardening](https://tryhackme.com/room/networkdevicehardening) room.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Which security model is being used to analyse the breach and defence strategies? | `Diamond Model` |
| Which defence capability is used to actively search for signs of malicious activity? | `Threat Hunting` |
| What are our main two infrastructure focuses? (Answer format: answer1 and answer2) | `Firewall and Honeypot` |
| Which firewall command is used to block traffic? | `Deny` |
| There is a flag in one of the stories. Can you find it? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
