# Advent of Cyber 2023 - Day 19: Memory Forensics

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Linux memory forensics with **Volatility 3** (`linux.*` plugins) to detect rogue cryptomining processes and extract plaintext credentials.

## Walkthrough
1. Analyze bash history from the memory dump:
   ```bash
   python3 vol.py -f memory.lime linux.bash
   ```
   Plaintext credentials are found: `ReindeerGames123`.
2. Enumerate running processes:
   ```bash
   python3 vol.py -f memory.lime linux.pslist
   ```
   Rogue miner identified running under PID `395`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Exposed password in bash history? | `ReindeerGames123` |
| PID of the miner process? | `395` |
