# Advent of Cyber 2023 - Day 20: Advent of Frostlings

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will be looking at how automated pipelines in for example GitLab can be abused to compromise software development and deployment.

1. What is the handle of the developer responsible for the merge changes?

   To view the merges, we navigate to the 'Merge Requests' tab and select the merged request.

   ![Merges](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D20_Merges.png)

   Looking at the commits, we can see that Frostlino authored both changes as well as the merge itself.

   ![Merge Yml](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D20_Merge_Yml.png)

   However, we need his handle, not just his username.

   ![Merge Handle](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D20_Merge_Handle.png)

   ><details><summary>Click for answer</summary>BadSecOps</details>

2. What port is the defaced calendar site server running on?

   Going back to the repository files, we can open the `.gitlab-ci.yml` file to see to port of the docker container used.

   ![Port](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D20_Port.png)

   ><details><summary>Click for answer</summary>9081</details>

3. What server is the malicious server running on?

   In the same file, we can see the name of the container image (and consequently the server software) used for the website.

   ><details><summary>Click for answer</summary>apache</details>

4. What message did the Frostlings leave on the defaced site?

   Going to the webpage with the port we just found reveals the defaced website and what is writen on it.

   ```html
   http://10.10.174.50:9081/
   ```

   ![Defaced Calendar](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D20_Defaced_Calendar.png)

   ><details><summary>Click for answer</summary>Frostlings rule</details>

5. What is the commit ID of the original code for the Advent Calendar site?

   Lets take a look at the commit section.

   ##
   
   **Note: We are looking for the code for the deployment pipeline. Not the code for the website itself.**

   ##

   ![Commits](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D20_Commits.png)

   The first couple commits are from Delf Lead who added the website and some other files. The last commit on december 6th is the one we could be looking for. It contains the deployment pipeline. Hopefully the original.

   ![Original Commit](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D20_Original_Commit.png)

   Here we can indeed see it contains the original pipeline code without the compromised webpage.

   In the top of the screen we can see what its commit ID is.

   ><details><summary>Click for answer</summary>986b7407</details>

If you enjoyed today's challenge, please check out the [Source Code Security](https://tryhackme.com/room/sourcecodesecurity) room.

Detective Frosteau believes it was an account takeover based on the activity. However, Tracy might have left some crumbs.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the handle of the developer responsible for the merge changes? | `BadSecOps` |
| What port is the defaced calendar site server running on? | `9081` |
| What server is the malicious server running on? | `apache` |
| What message did the Frostlings leave on the defaced site? | `Frostlings rule` |
| What is the commit ID of the original code for the Advent Calendar site? | `986b7407` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
