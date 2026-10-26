# Advent of Cyber 2024 - Day 20: Traffic Analysis

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

If you're nice, I'll have a surprise for you!

## Background

Command and Control (C2) traffic exhibits identifiable network patterns: beaconing intervals, payload size regularity, and persistent egress connections to foreign hosts.

Wireshark enables deep packet inspection (DPI) to dissect application protocols, isolate anomalous TCP streams, and reassemble transferred payloads to detect initial foothold execution.

## Walkthrough

**Step 1 — Filter Out Legitimate Traffic**

In Wireshark, filter out standard HTTP/HTTPS noise:
```wireshark
tcp && !tcp.port in {80, 443}
```

**Step 2 — Trace Attacker C2 Server**

Observe periodic TCP connections to external IP `10.10.123.224` on high port 8888.

**Step 3 — Follow TCP Stream**

Right-click the conversation and select **Follow > TCP Stream**:
- Client sends initial beacon: `I am in Mayor!`
- Server responds with first command: `whoami`

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What was the initial message sent by the agent to the C2? | `I am in Mayor!` |
| What is the IP address of the C2 server? | `10.10.123.224` |
| What was the first command sent by the attacker? | `whoami` |
