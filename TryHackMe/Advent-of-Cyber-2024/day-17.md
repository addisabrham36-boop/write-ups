# Advent of Cyber 2024 - Day 17: He analyzed and analyzed till his analyzer was sore!

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. Extract all the events from the cctv_feed logs. How many logs were captured associated with the successful login?

   First we should add the regex field extraction as mentioned in the text using the following regex command:

   ```cmd
   ^(?P<timestamp>\d+\-\d+\-\d+\s+\d+:\d+:\d+)\s+(?P<Event>(Login\s\w+|\w+))\s+(?P<user_id>\d+)?\s?(?P<UserName>\w+)\s+.*?(?P<Session_id>\w+)$
   ```

   Now we can filter the logs. Looking at the event type filter, we can see how many logs are associated with a successful login.

   ![Login](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_17_Login.png)

   ><details><summary>Click for answer</summary>642</details>

2. What is the Session_id associated with the attacker who deleted the recording?

   In the same filter, we can select the 'DeleteRecording' filter. Here we can see which Session_id is connected to this event.

   ![Session](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_17_Session.png)

   ><details><summary>Click for answer</summary>rij5uu4gt204q0d3eb7jj86okt</details>

3. What is the name of the attacker found in the logs, who deleted the CCTV footage?

   With the Session_id we can look for and IP address in the weblog logs with the following filter"

   ```cmd
   index=web_logs *rij5uu4gt204q0d3eb7jj86okt* | table _time clientip status uri file
   ```

   ![Ip](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_17_Ip.png)

   We can now look for more session ids associated with this IP address using the following:

   ```cmd
   index=web_logs *10.11.105.33*
   | stats count by status
   ```

   ![Sessions](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_17_Sessions.png)

   In the cctv log we can look which username is associated to both sessions ids we found.

   ```cmd
   index="cctv_feed" (Session_id = "lsr1743nkskt3r722momvhjcs3" OR Session_id = "b063lipf3rt6mclqen7ov9i9d8")
   ```

   ![Username](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_17_Username.png)

   ><details><summary>Click for answer</summary>mmalware</details>

4. Check out theSplunk: Data Manipulationroom to learn more about parsing and manipulating data in Splunk.

5. Good thing we had a backup of the CCTV application from yesterday. We got it running again in no time!

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Extract all the events from the cctv_feed logs. How many logs were captured associated with the successful login? | `642` |
| What is the Session_id associated with the attacker who deleted the recording? | `rij5uu4gt204q0d3eb7jj86okt` |
| What is the name of the attacker found in the logs, who deleted the CCTV footage? | `mmalware` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
