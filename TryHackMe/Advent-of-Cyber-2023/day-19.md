# Advent of Cyber 2023 - Day 19: CrypTOYminers Sing Volala-lala-latility

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will be looking at Volatility and how it can be used to read a memory dump file of a machine.

1. What is the exposed password that we find from the bash history output?

   First we must configure Volatility to be able to read the memory file. We can copy the pre-made Linux profile.

   ```cmd
   cp Desktop/Evidence/Ubuntu_5.4.0-163-generic_profile.zip ~/.local/lib/python2.7/site-packages/volatility/plugins/overlays/linux/
   vol.py --info | grep ubuntu
   ```

   ![Profile](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D19_Profile.png)

   Now using the following command, we can see which plugins we can use. We will be using the bash plugin for this question.

   ```cmd
   vol.py -f linux.mem --profile="LinuxUbuntu_5_4_0-163-generic_profilex64" linux_bash
   ```

   ![Bash History](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D19_Bash_History.png)
   
   ><details><summary>Click for answer</summary>NEhX4VSrN7sV</details>

2. What is the PID of the miner process that we find?

   For this we must use `linux_pslist` to view the processes running on the machine.

   ```cmd
   vol.py -f linux.mem --profile="LinuxUbuntu_5_4_0-163-generic_profilex64" linux_pslist
   ```

   ![Processes](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D19_Processes.png)

   ><details><summary>Click for answer</summary>10280</details>

3. What is the MD5 hash of the miner process?

   To extract both binaries, we first create a new folder called `extracted`. Then we run the `linux_procdump` plugin for both processes:

   ```cmd
   mkdir extracted
   vol.py -f linux.mem --profile="LinuxUbuntu_5_4_0-163-generic_profilex64" linux_procdump -D extracted -p 10280
   vol.py -f linux.mem --profile="LinuxUbuntu_5_4_0-163-generic_profilex64" linux_procdump -D extracted -p 10291
   ```

   ![Extract Binaries](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D19_Extract_Binaries.png)

   Now we can get the md5 hash of the miner binary using `md5sum`.

   ```cmd
   md5sum extracted/miner.10280.0x400000
   ```

   ![Hashes](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D19_Hashes.png)

   ><details><summary>Click for answer</summary>153a5c8efe4aa3be240e5dc645480dee</details>

4. What is the MD5 hash of the mysqlserver process?

   We can use the same command with the other binary.

   ```cmd
   md5sum extracted/mysqlserver.10291.0x400000
   ```

   ><details><summary>Click for answer</summary>c586e774bb2aa17819d7faae18dad7d1</details>

5. Use the command `strings extracted/miner.<PID from question 2>.0x400000 | grep http://`. What is the suspicious URL? (Fully defang the URL using CyberChef)

   Using the command with our miners PID gives us a suspicious url.

   ```cmd
   strings extracted/miner.10280.0x400000 | grep http://
   ```

   ![Url](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D19_Url.png)

   Using Cyberchef we can defang this URL.

   ![Url Defanged](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D19_Url_Defanged.png)

   ><details><summary>Click for answer</summary>hxxp[://]mcgreedysecretc2[.]thm</details>

6. After reading the elfie file, what location is the mysqlserver process dropped in on the file system?

   For this question we must look for any files related to cron jobs. This can be done with the `linux_enumerate_files` plugin.

   ```cmd
   vol.py -f linux.mem --profile="LinuxUbuntu_5_4_0-163-generic_profilex64" linux_enumerate_files | grep cron
   ```

   ![Cron Files](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D19_Cron_Files.png)

   Looks like the file of interest is located at `/var/spool/cron/crontabs/elfie`. Lets extract it.

   ```cmd
   vol.py -f linux.mem --profile="LinuxUbuntu_5_4_0-163-generic_profilex64" linux_find_file -i 0xffff9ce9b78280e8 -O elfie
   ```

   ![Extract File](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D19_Extract_File.png)

   Now we can read the file and see where the process is dropped.

   ![Location](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D19_Location.png)

   ><details><summary>Click for answer</summary>/var/tmp/.system-python3.8-Updates/mysqlserver</details>

If you enjoyed this task, feel free to check out the [Volatility](https://tryhackme.com/room/volatility) room.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the exposed password that we find from the bash history output? | `NEhX4VSrN7sV` |
| What is the PID of the miner process that we find? | `10280` |
| What is the MD5 hash of the miner process? | `153a5c8efe4aa3be240e5dc645480dee` |
| What is the MD5 hash of the mysqlserver process? | `c586e774bb2aa17819d7faae18dad7d1` |
| Use the command `strings extracted/miner.<PID from question 2>.0x400000 | grep http://`. What is the suspicious URL? (Fully defang the URL using CyberChef) | `hxxp[://]mcgreedysecretc2[.]thm` |
| After reading the elfie file, what location is the mysqlserver process dropped in on the file system? | `/var/tmp/.system-python3.8-Updates/mysqlserver` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
