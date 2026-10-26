# Advent of Cyber 2024 - Day 2: Log Analysis

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

One man's false positive is another man's indicator of compromise — Windows Event Logs tell the full story when you know what to look for.

## Background

Windows Event Logs capture every significant activity on a system: logons, process creation, service installs, policy changes, and more. Each event is tagged with an Event ID that gives us a quick shorthand for what happened. Event ID `4624` means a successful logon occurred — this is one of the most important IDs to hunt for during incident response.

Elastic/Kibana is a popular SIEM stack for centralising and searching these logs at scale. It ingests Windows Security logs (and many other sources) and lets analysts write powerful queries, build dashboards, and pivot across thousands of events in seconds. Without a SIEM, manually digging through `.evtx` files would be painfully slow.

The key workflow here is: filter by time window → narrow by Event ID → identify unusual accounts or source IPs → correlate timestamps → reconstruct the attacker's actions. Let's do exactly that.

## Walkthrough

**Step 1 — Open Elastic/Kibana and set the time range**

Log into the Kibana instance provided in the room. Navigate to **Discover** and set the time range to cover **December 1, 2024**.

**Step 2 — Filter for successful logon events**

Use a KQL filter to target Event ID 4624 (successful logon) in the Windows Security logs:

```kql
event.code: "4624"
```

Scroll through the results. We're looking for logon events from unexpected accounts or IP addresses outside business hours.

**Step 3 — Identify the compromised account and source IP**

The compromised account that stands out is `service_admin`. The source IP making the logon is:

```
10.0.255.1
```

**Step 4 — Note the suspicious logon timestamp**

The first suspicious logon timestamp for this account is:

```
Dec 1, 2024 08:54:39.000
```

**Step 5 — Find the PowerShell command executed**

Pivot to process creation events (Event ID 4688) or look at Sysmon logs for PowerShell activity correlated to this account:

```kql
event.code: "4688" and user.name: "service_admin"
```

The malicious PowerShell command executed was:

```powershell
Install-WindowsUpdate -AcceptAll -AutoReboot
```

**Step 6 — Identify the unusual port**

Review network connection events or firewall logs. The unusual port observed during this incident is `6791`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the compromised account name? | `service_admin` |
| What is the unusual port observed? | `6791` |
| What is the attacker's source IP address? | `10.0.255.1` |
| What is the timestamp of the first suspicious logon? | `Dec 1, 2024 08:54:39.000` |
| What PowerShell command was run? | `Install-WindowsUpdate -AcceptAll -AutoReboot` |
