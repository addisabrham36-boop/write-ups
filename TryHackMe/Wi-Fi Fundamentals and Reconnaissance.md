# Wi-Fi Fundamentals and Reconnaissance

{% embed url="https://tryhackme.com/room/wififundamentals" %}

An introduction to IEEE 802.11 wireless networking, frame architectures, beacon/probe dynamics, monitor mode operations, and wireless reconnaissance with the Aircrack-ng suite.

---

## 1. How Wi-Fi Operates

Wireless signals expand beyond physical corporate perimeters, making radio reconnaissance a primary vector:
- **SSID (Service Set Identifier):** The human-readable wireless network name.
- **BSSID (Basic Service Set Identifier):** The hardware MAC address of the Access Point's wireless radio.
- **ESSID (Extended Service Set Identifier):** An identifier representing multiple interconnected Access Points sharing a common network identity.

---

## 2. 802.11 Frame Hierarchy

IEEE 802.11 groups frames into three categories:
1. **Management Frames:** Network discovery, joining, and leaving.
   - **Beacon Frames:** Transmitted periodically by Access Points to announce their network capabilities and presence.
   - **Probe Request Frames:** Transmitted by client stations searching for previously joined networks or nearby APs.
   - **Association Frames:** Exchanged during mutual network negotiation.
2. **Control Frames:** Channel arbitration and transmission acknowledgment (RTS/CTS, ACK).
3. **Data Frames:** Encapsulated higher-layer network packets.

---

## 3. Monitor Mode & Network Discovery

Standard client cards run in **Managed Mode**, discarding frames not addressed to their hardware MAC. **Monitor Mode** instructs the radio to capture all raw frames propagating through the air on a given frequency channel.

```bash
# Kill interfering system network managers
sudo airmon-ng check kill

# Enable monitor mode
sudo airmon-ng start wlan0

# Sniff wireless traffic
sudo airodump-ng wlan0mon
```

---

## 4. Uncovering Hidden SSIDs

When an administrator disables SSID broadcast ("cloaked SSID"), the Access Point omits its name from Beacon frames. However, the SSID transmits in **cleartext** inside **Association Frames** whenever a client station associates or reconnects to the Access Point.

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | MAC address of an access point's radio | `BSSID` |
| **Task 3** | Management frame transmitted periodically by APs to advertise presence | `Beacon` |
| **Task 3** | Management frame transmitted by clients searching for networks | `Probe Request` |
| **Task 4** | Wireless card mode required to capture traffic not addressed to your device | `Monitor mode` |
| **Task 5** | Tool from aircrack-ng suite used to capture packets and enumerate APs | `airodump-ng` |
| **Task 6** | Frames from which hidden SSIDs are captured during reconnection | `Association frames` |
