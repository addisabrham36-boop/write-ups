# Advent of Cyber 2023 - Day 17: I Tawt I Taw A C2 Tat!

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Overview & Investigation Walkthrough

In this task we will be looking at SiLK and how we can use it to filter the traffic data files we obtained.

1. Which version of SiLK is installed on the VM?

   To get the version of SiLK that is installed we can use the config command:

   ```cmd
   silk_config -v
   ```

   ![D17 Silk Version](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D17_Silk_Version.png)

   ><details><summary>Click for answer</summary>3.19.1</details>

2. What is the size of the flows in the count records?

   To get the size of the flow, we can use the `rwfileinfo` command supplied by SiLK.

   ```cmd
   rwfileinfo suspicious-flows.silk
   ```

   ![D17 File Info](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D17_File_Info.png)

   ><details><summary>Click for answer</summary>11774</details>

3. What is the start time (sTime) of the sixth record in the file?

   For this we must look at the record data itself using `rwcut`. We can specify a couple of interesting columns (including sTime) and only show the first 6 records.

   ```cmd
   rwcut suspicious-flows.silk --fields=protocol,sIP,sPort,dIP,dPort,sTime --num-recs=6
   ```

   ![D17 Record 6](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D17_Record_6.png)

   ><details><summary>Click for answer</summary>2023/12/05T09:33:07.755</details>

4. What is the destination port of the sixth UDP record?

   For this we must use `rwfiltering` to filter the data before displaying it. The desired protocol is 17 (UDP).

   ```cmd
   rwfilter suspicious-flows.silk --protocol=17 --pass=stdout | rwcut --num-recs=6
   ```

   ![D17 Udp Records](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D17_Udp_Records.png)

   ><details><summary>Click for answer</summary>49950</details>

5. What is the record value (%) of the dport 53?

   To the get this answer we must use the `rwstats` command to get statistics on our data. Using dPort as our field of interest, we can use the following command:

   ```cmd
   rwstats suspicious-flows.silk --fields=dPort --values=records,packets,bytes,sIP-Distinct,dIP-Distinct --count=10
   ```

   ![D17 Statistics](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D17_Statistics.png)

   ><details><summary>Click for answer</summary>35.332088</details>

6. What is the number of bytes transmitted by the top talker on the network?

   We must modify our filter query we used before. The values listed starts sorting on the records, whereas we must sort by bytes sent.

   ```cmd
   rwstats suspicious-flows.silk --fields=sIP --values=bytes,records --count=10 --top
   ```

   ![D17 Top Talkers](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D17_Top_Talkers.png)

   ><details><summary>Click for answer</summary>735229</details>

7. What is the sTime value of the first DNS record going to port 53?

   For this we must filter the data with destination port as 53.

   ```cmd
   rwfilter suspicious-flows.silk --protocol=17 --dport=53 --pass=stdout | rwcut --num-recs=1
   ```

   ![D17 Dns](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D17_Dns.png)

   ><details><summary>Click for answer</summary>2023/12/08T04:28:44.825</details>

8. What is the IP address of the host that the C2 potentially controls? (In defanged format: 123[.]456[.]789[.]0 )

   If we use the following command to find out which ports are being used the most:

   ```cmd
   rwstats suspicious-flows.silk --fields=dPort --values=bytes -count=10

   rwfilter suspicious-flows.silk --aport=53 --pass=stdout | rwstats --fields=sIP,dIP --count=10
   ```

   ![D17 Possible C2](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D17_Possible_C2.png)
   
   We can see that port 53 (DNS) is of most interest to us. And the second command gives us which IPs are using these ports.

   To find out which one is the C2 and which one is the vulnerable machine. We must look for which machine sent data to port 53 using the following command:

   ```cmd
   rwfilter suspicious-flows.silk --saddress=175.175.173.221 --dport=53 --pass=stdout | rwcut --fields=sIP,sPort,dIP,dPort,sTime --num-recs=10

   rwfilter suspicious-flows.silk --saddress=175.219.238.243 --dport=53 --pass=stdout | rwcut --fields=sIP,sPort,dIP,dPort,sTime --num-recs=10
   ```

   ![D17 Possible C2 Origin](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D17_Possible_C2_Origin.png)

   It looks like our first IP is the compromised machine as it is the one sending data to port 53.

   ><details><summary>Click for answer</summary>175[.]175[.]173[.]221</details>

9. Which IP address is suspected to be the flood attacker? (In defanged format: 123[.]456[.]789[.]0 )

   The first image in the previous question also highlighted a lot of traffic on port 80.

   Using the following commands we can see which IPs interacted with these ports and which was the sender:

   ```cmd
   rwfilter suspicious-flows.silk --aport=80 --pass=stdout | rwstats --fields=sIP,dIP --count=10

   rwfilter suspicious-flows.silk --aport=80 --pass=stdout | rwstats --fields=sIP,dIP,dPort --count=10
   ```

   ![D17 Flood IP Origin](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D17_Flood_IP_Origin.png)

   ><details><summary>Click for answer</summary>175[.]215[.]236[.]223</details>

10. What is the sent SYN packet's number of records?
   
    Using the following commands we can see which flags are sent for which hosts:

    ```cmd
    rwfilter suspicious-flows.silk --saddress=175.215.236.223 --pass=stdout | rwcut --fields=sIP,dIP,dPort,sTime,Flags | head

    rwfilter suspicious-flows.silk --saddress=175.215.235.223 --pass=stdout | rwcut --fields=sIP,dIP,dPort,sTime,Flags | head
    ```

    ![D17 Syn Packets](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D17_Syn_Packets.png)

    Since we want the number of sent SYN packets by `175.215.236.223` we can use:

    ```cmd
    rwfilter suspicious-flows.silk --saddress=175.215.236.223 --pass=stdout | rwstats --fields=sIP,flags,dIP --count=10
    ```

    ![D17 Sent Syn](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2023/Advent_Of_Cyber_2023_D17_Sent_Syn.png)

    ><details><summary>Click for answer</summary>1658</details>

We've successfully analysed network flows to gain quick statistics. If you want to delve deeper into network packets and network data, you can look at the [Network Security and Traffic Analysis](https://tryhackme.com/module/network-security-and-traffic-analysis) module.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Which version of SiLK is installed on the VM? | `3.19.1` |
| What is the size of the flows in the count records? | `11774` |
| What is the start time (sTime) of the sixth record in the file? | `2023/12/05T09:33:07.755` |
| What is the destination port of the sixth UDP record? | `49950` |
| What is the record value (%) of the dport 53? | `35.332088` |
| What is the number of bytes transmitted by the top talker on the network? | `735229` |
| What is the sTime value of the first DNS record going to port 53? | `2023/12/08T04:28:44.825` |
| What is the IP address of the host that the C2 potentially controls? (In defanged format: 123[.]456[.]789[.]0 ) | `175[.]175[.]173[.]221` |
| Which IP address is suspected to be the flood attacker? (In defanged format: 123[.]456[.]789[.]0 ) | `175[.]215[.]236[.]223` |
| What is the sent SYN packet's number of records? | `1658` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
