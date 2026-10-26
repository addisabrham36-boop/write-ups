# Advent of Cyber 2024 - Day 2: One man's false positive is another man's potpourri.

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What is the name of the account causing all the failed login attempts?

   Filtering on the date from November 29th 0.00 to December 1st 9.30, we see one username responsible for many failed login attempts.

   ![Failed Attempts](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_2_Failed_Attempts.png)

   ><details><summary>Click for answer</summary>service_admin</details>

2. How many failed logon attempts were observed?

   To view how many failed attempts have been made, we should filter on `event.outcome` and `event.category`.

   ![Number](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_2_Number.png)

   ><details><summary>Click for answer</summary>6791</details>

3. What is the IP address of Glitch?

   Filtering of the user name and `event.category` = "authentication", and filtering the previously found IP we get the ip of the Glitch.

   ![Ip](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_2_Ip.png)

   ><details><summary>Click for answer</summary>10.0.255.1</details>

4. When did Glitch successfully logon to ADM-01? Format: MMM D, YYYY HH:MM:SS.SSS

   Now we can also filter furthe on hostname `ADM-01` and output `success`, the see when the successfull attempt by this IP address was made.

   ![Success](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_2_Success.png)

   ><details><summary>Click for answer</summary>Dec 1, 2024 08:54:39.000</details>

5. What is the decoded command executed by Glitch to fix the systems of Wareville?

   We can filter out any entries with a blank command value.

   ![Command](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_2_Command.png)

   Here we see the powershell command with a base64 encoded payload. We can decode this using Cyberchef. Make sure to also decode the text using `UTF-16LE (1200)` encoding.

   ![Decoded](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_2_Decoded.png)

   ><details><summary>Click for answer</summary>Install-WindowsUpdate -AcceptAll -AutoReboot</details>

6. If you enjoyed this task, feel free to check out theInvestigating with ELK 101room.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the name of the account causing all the failed login attempts? | `service_admin` |
| How many failed logon attempts were observed? | `6791` |
| What is the IP address of Glitch? | `10.0.255.1` |
| When did Glitch successfully logon to ADM-01? Format: MMM D, YYYY HH:MM:SS.SSS | `Dec 1, 2024 08:54:39.000` |
| What is the decoded command executed by Glitch to fix the systems of Wareville? | `Install-WindowsUpdate -AcceptAll -AutoReboot` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
