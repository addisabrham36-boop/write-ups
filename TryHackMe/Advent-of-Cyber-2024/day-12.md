# Advent of Cyber 2024 - Day 12: Race Conditions

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

It's all about timing!

## Background

Race conditions occur when system behavior depends on the timing or sequence of uncontrollable events. In web applications, Time-of-Check to Time-of-Use (TOCTOU) flaws allow attackers to execute concurrent actions before the application updates its internal state.

HTTP/2 multiplexing allows sending multiple requests over a single TCP stream. Attackers exploit this via "Last-Byte Synchronization" (single-packet attack), ensuring concurrent requests arrive and process in the exact same millisecond window.

## Walkthrough

**Step 1 — Identify the Coupon Redemption Logic**

Test the coupon redemption endpoint in Burp Suite:
```http
POST /api/redeem HTTP/2
Host: wareville.thm
Content-Type: application/json

{"coupon": "SANTA2024"}
```
Normal behavior allows redeeming the coupon once.

**Step 2 — Prepare Parallel Requests in Burp Repeater**

Create a tab group with 20 copies of the redemption request. Configure the group to use **Send group in parallel (last-byte sync)**.

**Step 3 — Execute the Race Condition Attack**

Transmit all 20 requests simultaneously. Because the database read occurs before the write locks commit, multiple redemptions succeed before the balance check trips. The application awards multiple items and prints the flag.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What HTTP protocol feature allows concurrent multiplexed streams? | `HTTP/2 Multiplexing` |
| What class of flaw occurs due to Time-of-Check to Time-of-Use timing? | `Race Condition` |
| What is the flag received after winning the race? | `THM{WON_THE_RACE_007}` |
