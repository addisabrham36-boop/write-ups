# Advent of Cyber 2023 - Day 21: Yule be Poisoned: A Pipeline of Insecure Code!

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will be looking at how we can posion a CI/CD pipeline using the permissions on different repositories.

1. What Linux kernel version is the Jenkins node?

   When recreating the steps outlined in the text we can see that we don't have permission to change the jenkins file in the repository.

   ![D21 Modify Jenkinsfile](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D21_Modify_Jenkinsfile.png)

   ![D21 Test Push](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D21_Test_Push.png)
   
   As mentioned we can try modifiying the makefile from the other repository
   
   ![D21 Modify Makefile](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D21_Modify_Makefile.png)

   We commit the changes to the remote repository with the following commands:

   ```cmd
   git add .
   git commit -m  "Trying something new"
   git push
   ```

   Now we run the main job in the Jenkins instance.
   
   ![D21 Run Test](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D21_Run_Test.png)

   Going back to the completed jobs we can see the output in the console output tab.
   
   ![D21 Test Result](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D21_Test_Result.png)

   This seems to have worked. Let's repeat the steps with the command to get the linux kernel version:

   ```cmd
   cat /proc/version
   ```

   Push the changes to the repository and re-run the job.
   
   ![D21 Kernel Version](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D21_Kernel_Version.png)
   
   ><details><summary>Click for answer</summary>5.4.0-1029-aws</details>

2. What value is found from /var/lib/jenkins/secret.key?

   We just need to add a different command to the makefile to read this file from the system.

   ```cmd
   cat /var/lib/jenkins/secret.key
   ```

   ![D21 Modify Makefile 2](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D21_Modify_Makefile_2.png)
   
   Push the changes and re-run the job.
   
   ```cmd
   git add .
   git commit -m "Whats the secret?"
   git push
   ```   
   
   ![D21 Secret Key](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D21_Secret_Key.png)

   ><details><summary>Click for answer</summary>90e748eafdd2af4746a5ef7941e63272f24f1e33a2882f614ebfa6742e772ba7</details>

Visit our [Discord](https://discord.gg/tryhackme)!

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What Linux kernel version is the Jenkins node? | `5.4.0-1029-aws` |
| What value is found from /var/lib/jenkins/secret.key? | `90e748eafdd2af4746a5ef7941e63272f24f1e33a2882f614ebfa6742e772ba7` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
