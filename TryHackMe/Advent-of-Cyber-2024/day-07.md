# Advent of Cyber 2024 - Day 7: Oh, no. I'M SPEAKING IN CLOUDTRAIL!

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What is the other activity made by the user glitch aside from the ListObject action?

   First we must extract the relevant data from the JSON file using jq.

   ```cmd
   jq -r '["Event_Time", "Event_Name", "User_Name", "Bucket_Name", "Key", "Source_IP"],(.Records[] | select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName=="wareville-care4wares") | [.eventTime, .eventName, .userIdentity.userName // "N/A", .requestParameters.bucketName // "N/A",.requestParameters.key // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t
   ```

   ![Activity](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_7_Activity.png)

   There is one other activity from user glitch.

   ><details><summary>Click for answer</summary>PutObject</details>

2. What is the source IP related to the S3 bucket activities of the user glitch?

   From the same results, we can see the IP address used for these activities.

   ><details><summary>Click for answer</summary>53.94.201.69</details>

3. Based on the eventSource field, what AWS service generates the ConsoleLogin event?

   First we must get all activity related to this glith user. To do this, we user `.userIdentity.userName == "glitch"` instead of the eventSource value.

   We should also add another column for the `eventSource` field.

   ```cmd
   jq -r '["Event_Time", "Event_Name", "User_Name", "Bucket_Name", "Event_Source", "Key", "Source_IP"],(.Records[] | select(.userIdentity.userName == "glitch") | [.eventTime, .eventName, .userIdentity.userName // "N/A", .requestParameters.bucketName // "N/A", .eventSource, .requestParameters.key // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t
   ```

   ![Service](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_7_Service.png)

   ><details><summary>Click for answer</summary>signin.amazonaws.com</details>

4. When did the anomalous user trigger the ConsoleLogin event?

   This can be found in the same place as the previous question.

   ><details><summary>Click for answer</summary>2024-11-28T15:21:54Z</details>

5. What was the name of the user that was created by the mcskidy user?

   From the previous question we saw mcskidy create a new user. Lets find out if this was indeed the user glitch.

   ```cmd
   jq -r '.Records[] | select(.eventSource == "iam.amazonaws.com" and .eventName == "CreateUser")' cloudtrail_log.json
   ```

   ![User](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_7_User.png)

   It seems it was mcskidy who created the account.

   ><details><summary>Click for answer</summary>glitch</details>

6. What type of access was assigned to the anomalous user?

   For this we should look for 'AttachUserPolicy' eventNames.

   ```cmd
   jq -r '.Records[] | select(.eventSource == "iam.amazonaws.com" and .eventName == "AttachUserPolicy")' cloudtrail_log.json 
   ```

   ![Policy](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_7_Policy.png)

   ><details><summary>Click for answer</summary>AdministratorAccess</details>

7. Which IP does Mayor Malware typically use to log into AWS?

   If we look for all entries coming from user 'mayor_malware', we can find out his IP address.

   ```cmd
   jq -r '["Event_Time", "Event_Name", "User_Name", "Event_Source", "Source_IP"],(.Records[] | select(.userIdentity.userName == "mayor_malware") | [.eventTime, .eventName, .userIdentity.userName // "N/A", .eventSource, .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t
   ```

   ![Ip](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_7_Ip.png)

   ><details><summary>Click for answer</summary>53.94.201.69</details>

8. What is McSkidy's actual IP address?

   We can do the same with mcskidy's account. Below the different IP, we can see her actual IP.

   ```cmd
   jq -r '["Event_Time", "Event_Name", "User_Name", "Event_Source", "Source_IP"],(.Records[] | select(.userIdentity.userName == "mcskidy") | [.eventTime, .eventName, .userIdentity.userName // "N/A", .eventSource, .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t
   ```

   ![Mcskidy](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_7_Mcskidy.png)

   ><details><summary>Click for answer</summary>31.210.15.79</details>

9. What is the bank account number owned by Mayor Malware?

   To get this information from the bank, we can further filter on 'Mayor Malware'.

   ```cmd
   grep INSERT | grep Mayor rds.log
   ```

   ![Bank](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_7_Bank.png)

   ><details><summary>Click for answer</summary>2394 6912 7723 1294</details>

10. Want to learn more about log analysis and how to interpret logs from different sources? Check outthe Log Universeroom!

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the other activity made by the user glitch aside from the ListObject action? | `PutObject` |
| What is the source IP related to the S3 bucket activities of the user glitch? | `53.94.201.69` |
| Based on the eventSource field, what AWS service generates the ConsoleLogin event? | `signin.amazonaws.com` |
| When did the anomalous user trigger the ConsoleLogin event? | `2024-11-28T15:21:54Z` |
| What was the name of the user that was created by the mcskidy user? | `glitch` |
| What type of access was assigned to the anomalous user? | `AdministratorAccess` |
| Which IP does Mayor Malware typically use to log into AWS? | `53.94.201.69` |
| What is McSkidy's actual IP address? | `31.210.15.79` |
| What is the bank account number owned by Mayor Malware? | `2394 6912 7723 1294` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
