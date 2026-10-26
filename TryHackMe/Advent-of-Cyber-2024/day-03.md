# Advent of Cyber 2024 - Day 3: Even if I wanted to go, their vulnerabilities wouldn't allow it.

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. BLUE: Where was the web shell uploaded to?Answer format:/directory/directory/directory/filename.php

   After accessing the log database, we change to the correct time frame.

   Now we can search for "shell.php" in the message.

   ![Message](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_3_Message.png)

   ><details><summary>Click for answer</summary>/media/images/rooms/shell.php</details>

2. BLUE: What IP address accessed the web shell?

   In the results we can see multiple IPs. One of which is malicious.

   ![Rce](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_3_Rce.png)

   That is the on where we can see a file upload abuse is used.

   ><details><summary>Click for answer</summary>10.11.83.34</details>

3. RED: What is the contents of the flag.txt?

   First, we need access to the account to upload our shell. At the login screen we can try logging various common credentials. One of these seems to work.

   ><details><summary>Click for hint</summary>admin@frostypines.thm : admin</details>

   ![Login](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_3_Login.png)

   On the admin dashboard, we see a feature to add a new room. Perhaps this lets us upload an image (our shell).

   ![New Room](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_3_New_Room.png)

   We can indeed upload an image here. Lets create our payload and try to upload it here. It may filter the extension, so we will see.

   ![Shell](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_3_Shell.png)

   ![Upload](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_3_Upload.png)

   ![Uploaded](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_3_Uploaded.png)

   Seems we were successfull! Let's try our shell.

   ![Web Shell](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_3_Web_Shell.png)

   Success! Now we can look for the flag and read its contents.

   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_3_Flag.png)

   ><details><summary>Click for answer</summary>THM{Gl1tch_Was_H3r3}</details>

4. If you liked today's task, you can learn how to harness the power ofadvanced ELK queries.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| BLUE: Where was the web shell uploaded to?Answer format:/directory/directory/directory/filename.php | `/media/images/rooms/shell.php` |
| BLUE: What IP address accessed the web shell? | `10.11.83.34` |
| RED: What is the contents of the flag.txt? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
