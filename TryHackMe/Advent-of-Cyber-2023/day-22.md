# Advent of Cyber 2023 - Day 22: Jingle Your SSRF Bells: A Merry Command & Control Hackventure

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will exploit a SSRF vulnerability in the C2 server of McGreedy to gain access to the server and remove the compromised machines.

1. Is SSRF the process in which the attacker tricks the server into loading only external resources (yea/nay)?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>Nay</details>

2. What is the C2 version?

   On the homepage we are greeted with a login portal. At the bottom is a link to the API documention which could be usefull.

   ![D22 C2 Login Screen](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D22_C2_Login_Screen.png)

   ![D22 Interesting File](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D22_Interesting_File.png)

   On th page we can see which URL to use to access the resources. We can replace the external url with `file:////`. Which should let us access system files.

   ![D22 Ssrf Exploit](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D22_Ssrf_Exploit.png)

   We can check to see if it works by looking for the index.php page. 

   Normally this would be in the root folder of the webserver. On linux systems this is usually `/var/www/html`.

   ```http
   http://10.10.150.36/getClientData.php?url=file:////var/www/html/index.php
   ```

   ![D22 Index](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D22_Index.png)

   Looks like we indeed get back the contents of the file. Lets try and see if the config.php file is in the same folder.

   ```http
   http://10.10.150.36/getClientData.php?url=file:////var/www/html/config.php
   ```

   ![D22 Config](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D22_Config.png)

   Success! Now we have the credentials to login to the server. 

   In the bottom right corner of the C2 dashboard we can see the version of the server.

   ><details><summary>Click for answer</summary>1.1</details>

4. What is the username for accessing the C2 panel?

   This was found in the previous question in the config.php file.

   ><details><summary>Click for answer</summary>mcgreedy</details>

5. What is the flag value after accessing the C2 panel?

   After logging into the server, we can see the flag at the top of the screen.

   ![D22 Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D22_Flag.png)

   ><details><summary>Click for answer</summary>THM{EXPLOITED_31001}</details>

6. What is the flag value after stopping the data exfiltration from the McSkidy computer?

   Under the "Hackes Users Information" tab on the dashboard we can see are the compromised machines.

   ![D22 Assets](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D22_Assets.png)

   To get our flag we must remove the machine of McSkidy.

   ![D22 Removed](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D22_Removed.png)

   ><details><summary>Click for answer</summary>THM{AGENT_REMOVED_1001}</details>

If you enjoyed this task, feel free to check out the [SSRF](https://tryhackme.com/room/ssrfqi) room.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Is SSRF the process in which the attacker tricks the server into loading only external resources (yea/nay)? | `Nay` |
| What is the C2 version? | `1.1` |
| What is the username for accessing the C2 panel? | `mcgreedy` |
| What is the flag value after accessing the C2 panel? | `flag{hidden}` |
| What is the flag value after stopping the data exfiltration from the McSkidy computer? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
