# Advent of Cyber 2024 - Day 24: You can’t hurt SOC-mas, Mayor Malware!

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What is the flag?

   After starting the challenge.sh script, we can see various windows pop up. One of them is the interface for the lights, but nothing works sadly.

   ![Light Interface](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_24_Light_Interface.png)

   Lets open the capture file in Wireshark and investigate. Since we are looking at packets related to MQTT we can add a filter on `mqtt`.

   In several packets we can see a command beiing sent. However, it is encoded. 

   ![Message](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_24_Message.png)

   Lets try to decode this message to find the information we need. We can use CyberChef and decode from Bae64.

   ![Base64](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_24_Base64.png)

   Looks like this is related to the lights. If we use this as our message/topic together with the `on` command, we should be able to turn the lights back on.

   ```cmd
   mosquitto_pub -h localhost -t d2FyZXZpbGxl/Y2hyaXN0bWFzbGlnaHRz -m on
   ```

   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_24_Flag.png)

   It worked! The lights are back on and we just got the flag for the final day in the advent of cyber!

   ><details><summary>Click for answer</summary>THM{Ligh75on-day54ved}</details>

2. If you enjoyed this task, feel free to check out theWiresharkmodule.

### Thank you, and congratulations!

1. What is the flag you get at the end of thesurvey?

   ><details><summary>Click for answer</summary>THM{we_will_be_back_in_2025}</details>

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the flag? | `flag{hidden}` |
| If you enjoyed this task, feel free to check out theWiresharkmodule. | `flag{hidden}` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
