# Advent of Cyber 2023 - Day 2: O Data, All Ye Faithful

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will be looking at a captured network traffic packet and analyse its contents using Jupyter Notebooks.

Open the notebook "Workbook" located in the directory "4_Capstone" on the VM. Use what you have learned today to analyse the packet capture.

1. How many packets were captured (looking at the PacketNumber)?

   We can use the `count()` function of pandas on our `df` variable. If we define the PacketNumber column, we get only that result.

   ```cmd
   df['PacketNumber'].count()
   ```

   ![Packets](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D2_Packets.png)

   ><details><summary>Click for answer</summary>100</details>

2. What IP address sent the most amount of traffic during the packet capture?

   For this we will first group our data on the Source column since we want to know the sending IP.

   We then us the `size()` command to count the number of times the IP address is listed. 

   Lastly, we can also sort the values on descending size to get the our answer on top.

   ```cmd
   df.groupby(['Source']).size().sort_values(ascending=False)
   ```

   ![IP](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D2_IP.png)

   ><details><summary>Click for answer</summary>10.10.1.4</details>

3. What was the most frequent protocol?

   This time we can simply specify the column we are interested in, but we need to count the number each value within that column is listed.

   ```cmd
   df['Protocol'].value_counts().sort_values(ascending=False)
   ```

   ![Protocol](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D2_Protocol.png)

   ><details><summary>Click for answer</summary>ICMP</details>

If you enjoyed today's task, check out the [Intro to Log Analysis](https://tryhackme.com/room/introtologanalysis) room.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| How many packets were captured (looking at the PacketNumber)? | `100` |
| What IP address sent the most amount of traffic during the packet capture? | `10.10.1.4` |
| What was the most frequent protocol? | `ICMP` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
