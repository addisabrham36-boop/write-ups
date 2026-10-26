# Advent of Cyber 2023 - Day 17: Traffic Analysis

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Analyzing high-volume network flow data (NetFlow / IPFIX) using **SiLK** (System for Internet-Level Knowledge) command-line tools.

## Walkthrough
Filter NetFlow records using `rwfilter`:
```bash
# Filter for outgoing flows to foreign IP addresses
rwfilter --proto=6 --pass=stdout flows.silk | rwstats --fields=dip --top=10
```
Querying flows identifies suspicious periodic C2 beaconing on high ephemeral ports and uncovers data exfiltration channels.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| SiLK traffic analysis completion flag | `THM{SILK_NETFLOW_ANALYST}` |
