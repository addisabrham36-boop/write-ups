# Advent of Cyber 2022 - Day 2: Log Analysis

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Analyzing raw web server access logs to reconstruct a security breach. We learn how to parse Common Log Format (CLF) and Combined Log Format entries using Linux core utilities: `head`, `tail`, `wc`, `cat`, and `grep`.

## Walkthrough
Connect to the target terminal and list files in the current directory:
```bash
ls -lah
```
We identify two log files: `sshd.log` and `webserver.log`. Inspecting `webserver.log`:
```bash
# Count total entries
wc -l webserver.log

# Search for sensitive files accessed
grep -inE "list|santa|stolen" webserver.log
```
The output reveals suspicious GET requests originating from IP `10.10.249.191` requesting `/santaslist.txt` on a Friday:
```text
10.10.249.191 - - [18/Nov/2022:10:14:32 +0000] "GET /santaslist.txt HTTP/1.1" 200 4521
```
Searching for the platform flag format:
```bash
grep -i "THM{" webserver.log
```

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Use `ls` to list the files. How many log files are present? | `2` |
| What is the name of the web server log file? | `webserver.log` |
| On what day was Santa's naughty and nice list stolen? | `Friday` |
| What is the IP address of the attacker? | `10.10.249.191` |
| What is the name of the important list that the attacker stole? | `santaslist.txt` |
| Look through the log files for the flag: | `THM{STOLENSANTASLIST}` |
