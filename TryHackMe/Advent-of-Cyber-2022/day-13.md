# Advent of Cyber 2022 - Day 13: Packet Analysis

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Analyzing a captured packet trace (`capture.pcap`) with **Wireshark** to detect malware staging, HTTP object transfers, and attacker Command & Control communication.

## Walkthrough
1. Open `capture.pcap` in Wireshark.
2. Navigate to **Statistics > Protocol Hierarchy** to observe the distribution of packet types.
3. Check **Statistics > Conversations** to identify high-volume communication:
   - Port `80` (`http`) transferred over 1,000 packets between the client and an external CDN.
4. Filter HTTP requests:
   ```wireshark
   http.request
   ```
   The infected host queries `cdn[.]bandityeti[.]thm` to download `mysterygift.exe`.
5. Export the transferred file: **File > Export Objects > HTTP > Save mysterygift.exe**.
6. Calculate its cryptographic hash:
   ```bash
   sha256sum mysterygift.exe
   ```

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Which port number received more than 1000 packets? | `80` |
| Service name of the used protocol receiving >1000 packets? | `http` |
| Domain address hosting malicious file (defanged)? | `cdn[.]bandityeti[.]thm` |
