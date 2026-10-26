# Advent of Cyber 2024 - Day 14: Even if we're horribly mismanaged, there'll be no sad faces on SOC-mas!

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What is the name of the CA that has signed the Gift Scheduler certificate?

   After setting up our browser to proxy to burpsuite and opening burpsuite, we should add the entries to our host file. One to resolve the FQDN of the gift scheduler to its IP address. And one to point 'wareville-gw' to our own attackbox IP.

   ![Hosts](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_14_Hosts.png)

   We can now access the dashboard after loggin in on gift-scheduler.thm.

   ![Dashboard](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_14_Dashboard.png)

   I got no warning about the self-signed certificate, but seeing the answer has three letters, I can make an educated guess.

   ><details><summary>Click for answer</summary>THM</details>

2. Look inside the POST requests in the HTTP history. What is the password for thesnowballelf account?

   Set up a listener in the Burpsuite proxy settings. IP: 10.11.101.240 and port: 8080.

   ![Listener](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_14_Listener.png)

   Unfortunately, I realized I need to run a script that is located on the attackbox. So redo everything on the attackbox then execute the script.

   ![Requests](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_14_Requests.png)

   We can filter the results on the account name.

   ![Snowballelf](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_14_Snowballelf.png)

   ><details><summary>Click for answer</summary>c4rrotn0s3</details>

3. Use the credentials for any of the elves to authenticate to the Gift Scheduler website. What is the flag shown on the elves’ scheduling page?

   With these credentials, we can login to the dashboard.

   ```cmd
   Username: snowballelf
   Password: c4rrotn0s3
   ```

   ![Flag 1](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_14_Flag_1.png)

   ><details><summary>Click for answer</summary>THM{AoC-3lf0nth3Sh3lf}</details>

4. What is the password for Marta May Ware’s account?

   After a while her account should be captured. We can filter the list the same way we did before.

   ![Marta](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_14_Marta.png)

   ><details><summary>Click for answer</summary>H0llyJ0llySOCMAS!</details>

5. Mayor Malware finally succeeded in his evil intent: with Marta May Ware’s username and password, he can finally access the administrative console for the Gift Scheduler. G-Day is cancelled!What is the flag shown on the admin page?

   Now that we have some admin credentials, we can login to the admin dashboard to find our second flag.

   ![Flag 2](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_14_Flag_2.png)

   ><details><summary>Click for answer</summary>THM{AoC-h0wt0ru1nG1ftD4y}</details>

6. If you enjoyed this task, feel free to check out theBurp Suitemodule.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the name of the CA that has signed the Gift Scheduler certificate? | `THM` |
| Look inside the POST requests in the HTTP history. What is the password for thesnowballelf account? | `c4rrotn0s3` |
| Use the credentials for any of the elves to authenticate to the Gift Scheduler website. What is the flag shown on the elves’ scheduling page? | `flag{hidden}` |
| What is the password for Marta May Ware’s account? | `H0llyJ0llySOCMAS!` |
| Mayor Malware finally succeeded in his evil intent: with Marta May Ware’s username and password, he can finally access the administrative console for the Gift Scheduler. G-Day is cancelled!What is the flag shown on the admin page? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
