# Advent of Cyber 2023 - Day 7: Log Analysis

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
High-speed log forensics in Linux CLI using `cut`, `awk`, `sort`, and `uniq` to detect malicious external web proxy requests.

## Walkthrough
Parse `access.log` to identify high-frequency requests, abnormal User-Agents, and outbound exfiltration:
```bash
# Count requests per client IP
cut -d ' ' -f 1 access.log | sort | uniq -c | sort -nr

# Extract requested domains and status codes
awk '{print $7, $9}' access.log | grep -E "\.ru|\.xyz|\.cc"
```
Following the timeline of compromised internal endpoints exposes the investigator reward flag: `THM{a_gift_for_you_awesome_analyst!}`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Flag found at the end of the log analysis? | `THM{a_gift_for_you_awesome_analyst!}` |
