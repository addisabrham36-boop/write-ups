# Advent of Cyber 2024 - Day 17: Splunk

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

He analyzed and analyzed till his analyzer was sore!

## Background

Splunk is a leading SIEM platform that ingests, indexes, and analyzes machine-generated telemetry. Splunk Search Processing Language (SPL) allows analysts to query and correlate millions of events in real time.

Key SPL syntax and commands:
- `index=`: Specifies which data repository to query.
- `stats`: Calculates counts, averages, and statistical aggregations.
- `table`: Formats specified fields into structured table views.
- `sort`: Orders results ascending or descending.

## Walkthrough

**Step 1 — Search for Authentication Failures**

Query failed logon events (EventCode 4625) in the Windows index:
```splunk
index=windows EventCode=4625
| stats count by Account_Name, Source_Network_Address
| sort -count
```

**Step 2 — Trace Process Spawning Activity**

Query process creation logs (EventCode 4688) for PowerShell execution:
```splunk
index=windows EventCode=4688 New_Process_Name="*powershell.exe*"
| table _time, Account_Name, CommandLine
```

**Step 3 — Identify Suspicious Script Block**

Correlate execution timestamps to find encoded PowerShell commands and reveal the investigator flag.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What Splunk command calculates summary statistics? | `stats` |
| What Windows EventCode tracks failed logon attempts in Splunk? | `4625` |
| What is the flag retrieved from the Splunk investigation? | `THM{SPLUNK_THREAT_HUNTING_SUCCESS}` |
