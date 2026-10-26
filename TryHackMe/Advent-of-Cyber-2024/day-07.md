# Advent of Cyber 2024 - Day 7: AWS CloudTrail

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

Oh, the weather outside is frightful, but the logs are so delightful!

## Background

AWS CloudTrail continuously monitors and records API activity across an AWS infrastructure. Every action taken by a user, IAM role, or AWS service is recorded as a JSON event in CloudTrail log files. In incident response, CloudTrail is the primary source of truth for cloud breaches.

Command-line tools like `jq` are essential for parsing massive CloudTrail log files. `jq` slices, filters, maps, and transforms JSON structured logs rapidly without requiring a heavyweight SIEM. Searching for high-risk API calls such as `PutObject` (file uploads/exfiltration), `ConsoleLogin`, and unauthorized `Describe` or `Get` operations allows responders to reconstruct the adversary's cloud attack chain.

## Walkthrough

**Step 1 — Inspect CloudTrail Log Structure**

Examine the raw log file with `jq` to understand its schema:
```bash
cat cloudtrail.json | jq '.Records[0]'
```

**Step 2 — Identify Suspicious S3 Activity**

Filter for S3 object creation and upload events (`PutObject`), which indicate potential data staging or exfiltration:
```bash
cat cloudtrail.json | jq '.Records[] | select(.eventName=="PutObject") | {time: .eventTime, ip: .sourceIPAddress, user: .userIdentity.arn}'
```

**Step 3 — Trace Attacker IP and Sign-In Events**

Correlate the suspicious source IP `53.94.201.69` across all logged API calls:
```bash
cat cloudtrail.json | jq '.Records[] | select(.sourceIPAddress=="53.94.201.69") | {event: .eventName, time: .eventTime, service: .eventSource}'
```

Identify the initial authentication event source:
```bash
cat cloudtrail.json | jq '.Records[] | select(.eventName=="ConsoleLogin") | .eventSource'
```
Result shows sign-ins originating from `signin.amazonaws.com`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the API call used to upload files to S3? | `PutObject` |
| What is the IP address of the attacker? | `53.94.201.69` |
| What was the event source for the signin event? | `signin.amazonaws.com` |
