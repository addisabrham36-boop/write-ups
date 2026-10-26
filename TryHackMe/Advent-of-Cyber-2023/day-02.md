# Advent of Cyber 2023 - Day 2: Log Analysis

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Data science in cybersecurity: Using Python, Pandas, and Jupyter Notebooks to parse PCAP-derived CSV telemetry, calculate summary statistics, and visualize anomalies.

## Walkthrough
Launch the Jupyter Notebook environment and run the provided analysis script:
```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv('network_traffic.csv')

# 1. Total packet count
print("Total Packets:", len(df))

# 2. Top talker IP address
print("Top Source IP:", df['Source'].value_counts().idxmax())

# 3. Protocol frequency breakdown
print("Top Protocol:", df['Protocol'].value_counts().idxmax())
```
Output results:
- Total packet rows: `100`
- Top active talker: `10.10.1.4`
- Dominant protocol: `ICMP`

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| How many packets were captured (looking at PacketNumber)? | `100` |
| What IP address sent the most amount of traffic? | `10.10.1.4` |
| What was the most frequent protocol? | `ICMP` |
