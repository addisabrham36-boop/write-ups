# Advent of Cyber 2024 - Day 20: If you utter so much as one packet…

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What was the first message the payload sent to Mayor Malware’s C2?

   After opening the pcap file, we should filter on packet originating from Marta's machine and protocol type (HTTP).

   ```cmd
   ip.src==10.10.229.217 and http
   ```

   ![Message](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_20_Message.png)

   ><details><summary>Click for answer</summary>I am in Mayor!</details>

2. What was the IP address of the C2 server?

   From the previous image, we can clearly see what the destination IP is of these packets.

   ><details><summary>Click for answer</summary>10.10.123.224</details>

3. What was the command sent by the C2 server to the target machine?

   For this we should look at the packets related to the GET /command stream.

   ![Command](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_20_Command.png)

   ><details><summary>Click for answer</summary>whoami</details>

4. What was the filename of the critical file exfiltrated by the C2 server?

   Here we also follow the http stream, but for the POST /exfiltrate request.

   ![File](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_20_File.png)

   ><details><summary>Click for answer</summary>credentials.txt</details>

5. What secret message was sent back to the C2 in an encrypted format through beacons?

   ![Packets](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_20_Packets.png)
   
   In the exfiltration message, we can see which encryption and key is used. In the beacon packets, we can actually find the secret.

   ![Secret](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_20_Secret.png)

   Using CyberChef, we can decrypt this message. 

   Use AES decryption with ECB mode.

   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_20_Flag.png)

   ><details><summary>Click for answer</summary>THM_Secret_101</details>

6. Learn more about WireShark in ourWireshark: Traffic Analysisroom.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What was the first message the payload sent to Mayor Malware’s C2? | `I am in Mayor!` |
| What was the IP address of the C2 server? | `10.10.123.224` |
| What was the command sent by the C2 server to the target machine? | `whoami` |
| What was the filename of the critical file exfiltrated by the C2 server? | `credentials.txt` |
| What secret message was sent back to the C2 in an encrypted format through beacons? | `THM_Secret_101` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
