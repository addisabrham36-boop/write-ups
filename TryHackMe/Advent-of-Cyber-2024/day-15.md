# Advent of Cyber 2024 - Day 15: Be it ever so heinous, there's no place like Domain Controller.

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. Use the "Security" tab within Event Viewer to answer questions 1 and 2.

2. On what day was Glitch_Malware last logged in?Answer format: DD/MM/YYYY

   To answer our first question, we should first filter the Security eventlog by eventID 4624. Then we can search for username "Glitch_Malware" for any logon attempts by this account.

   ![Logon](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_15_Logon.png)

   ><details><summary>Click for answer</summary>07/11/2024</details>

3. What event ID shows the login of the Glitch_Malware user?

   This is the same as the eventID we used to filter on successfull logon attempts.

   ><details><summary>Click for answer</summary>4624</details>

4. Read the PowerShell history of the Administrator account. What was the command that was used to enumerate Active Directory users?

   We can look for the PowerShell command history in the following file: "C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt"

   ![Powershell](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_15_Powershell.png)

   ><details><summary>Click for answer</summary>Get-ADUser -Filter * -Properties MemberOf | Select-Object Name</details>

5. Look in the PowerShell log file located inApplication and Services Logs -> Windows PowerShell. What was Glitch_Malware's set password?

   Make sure to look in the correct log file!

   In this log, look for any entries related to "password". Check entries related to the Glitch_Malware account.

   ![Password](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_15_Password.png)

   ><details><summary>Click for answer</summary>SuperSecretP@ssw0rd!</details>

6. Review the Group Policy Objects present on the machine. What is the name of the installed GPO?

   We can view these GPOs through the Group Policy Management Window or with Powershell.

   In the GPM window we can see the installed GPOs under: Domains -> wareville.thm -> GPOs in wareville.thm

   With PowerShell we can use `Get-GPO -All`.

   ![Gpo](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_15_Gpo.png)

   ><details><summary>Click for answer</summary>Malicious GPO - Glitch_Malware Persistence</details>

7. If you enjoyed this task, feel free to check out theActive Directory Hardeningroom.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Use the "Security" tab within Event Viewer to answer questions 1 and 2. | `07/11/2024` |
| What event ID shows the login of the Glitch_Malware user? | `4624` |
| Read the PowerShell history of the Administrator account. What was the command that was used to enumerate Active Directory users? | `Get-ADUser -Filter * -Properties MemberOf | Select-Object Name` |
| Look in the PowerShell log file located inApplication and Services Logs -> Windows PowerShell. What was Glitch_Malware's set password? | `SuperSecretP@ssw0rd!` |
| Review the Group Policy Objects present on the machine. What is the name of the installed GPO? | `Malicious GPO - Glitch_Malware Persistence` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
