# Advent of Cyber 2023 - Day 12: Sleighing Threats, One Layer at a Time

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will be looking at how various layers can be combined to create a secure environment.

1. What is the default port for Jenkins?

   The answer to this question can be found in the text.

   ><details><summary>Click for answer</summary>8080</details>

2. What is the password of the user tracy?

   First we must head towards to script page on the Jenkins instance. Then we setup a netcat listener on our machine using: `nc -nlvp 1337`.

   Now we copy the script snippet from the text and paste it into jenkins. Don't forget to add you IP and port.

   ![Jenkins Script](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D12_Jenkins_Script.png)

   Now we can simply click run and we should get a web shell.

   To get the password, we should lookup the backup script and its contents.

   ```cmd
   ls /opt/scipts
   cat /opt/scripts/backup.sh
   ```

   ![Backup Script](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D12_Backup_Script.png)

   ><details><summary>Click for answer</summary>13_1n_33</details>

3. What's the root flag?

   To get root flag we must elevate our privileges. From the backup file, we found credentials for the user Tracy. Lets ssh into the machine with Tracys credentials.

   After login in, we can run `sudo -l` to find out which commands the user is allowed to run.

   ![Tracy Sudo](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D12_Tracy_Sudo.png)

   Looks like tract is allowed to effectively run all commands with sudo. So we can simply switch to the root user with `sudo -i` or `sudo su`.

   ![Root](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D12_Root.png)

   Now that we are root, we can search for our root flag.

   ![Root Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D12_Root_Flag.png)

   ><details><summary>Click for answer</summary>ezRo0tW1thoutDiD</details>

5. What is the error message when you login as tracy again and try sudo -l after its removal from the sudoers group?

   Lets hop into our admin terminal to remove the user tracy from the sudoers file.

   ```cmd
   sudo deluser tracy sudo

   sudo -l -U tracy
   ```

   ![Remove Sudo](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D12_Remove_Sudo.png)

   We see tracy has now been removed. Running `sudo -l` on tracys ssh terminal should give us an error message.

   ![Tracy Error](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D12_Tracy_Error.png)

   ><details><summary>Click for answer</summary>Sorry, user tracy may not run sudo on jenkins.</details>

6. What's the SSH flag?

   Our next step is to disable the user of ssh passwords by modifying the ssh config file.

   In our terminal we open the following file:

   ```cmd
   sudo nano /etc/ssh/sshd_config
   ```

   And remove the include line.

   ![Ssh 1](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D12_Ssh_1.png)

   And add the password allowed line.

   ![Ssh 2](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D12_Ssh_2.png)

   Trying to log back into tracys account via ssh shouldn't work anymore.

   ![Ssh Error](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D12_Ssh_Error.png)

   The flag can be found in the ssh config file.

   ><details><summary>Click for answer</summary>Ne3d2SecureTh1sSecureSh31l</details>

7. What's the Jenkins flag?

   For our last flag, we must enable the Jenkins log in screen. To do so we open (with sudo) the jenkins config file from our admin terminal.

   ```cmd
   cd /var/lib/jenkins
   ls -lh
   sudo nano config.xml.bak
   ```

   ![Jenkins Files](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D12_Jenkins_Files.png)

   We must now remove the "!--" and "--" for both authorizationStrategy and securityRealm (The flag can be found in this document).
   
   ![Jenkins Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D12_Jenkins_Flag.png)

   Now we must replace `config.xml` with `config.xml.bak`.

   ```cmd
   sudo mv config.xml config.xml.bak1
   sudo mv config.xml.bak config.xml
   ```

   Lastly, we must restart the Jenkins instance using: `sudo systemctl restart jenkins`.

   ![Jenkins Restart](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D12_Jenkins_Restart.png)

   Now we are greeted with a login screen.

   ![Jenkins Login](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D12_Jenkins_Login.png)

   ><details><summary>Click for answer</summary>FullTrust_has_n0Place1nS3cur1ty</details>

If you enjoyed this room, please check out our SOC Level 1 learning path.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the default port for Jenkins? | `8080` |
| What is the password of the user tracy? | `13_1n_33` |
| What's the root flag? | `ezRo0tW1thoutDiD` |
| What is the error message when you login as tracy again and try sudo -l after its removal from the sudoers group? | `Sorry, user tracy may not run sudo on jenkins.` |
| What's the SSH flag? | `Ne3d2SecureTh1sSecureSh31l` |
| What's the Jenkins flag? | `FullTrust_has_n0Place1nS3cur1ty` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
