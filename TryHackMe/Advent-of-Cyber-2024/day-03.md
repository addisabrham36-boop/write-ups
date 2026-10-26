# Advent of Cyber 2024 - Day 3: Log Analysis

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

Even if the attacker wanted to hide, their vulnerabilities wouldn't allow it — the ELK stack sees everything.

## Background

Elastic SIEM (also called the ELK Stack — Elasticsearch, Logstash, Kibana) is a widely-deployed open-source solution for centralising security logs. Logs flow from endpoints and network devices through Logstash (or Beats agents), get stored in Elasticsearch, and are queried and visualised in Kibana. This setup gives defenders a single pane of glass over a sprawling infrastructure.

KQL (Kibana Query Language) is the query syntax used to filter log data in Kibana's Discover view. It supports field-level filtering (`field: "value"`), boolean logic (`and`, `or`, `not`), and wildcard matching. Knowing a handful of KQL patterns unlocks the ability to rapidly triage thousands of events.

In this room we pivot through authentication logs to reconstruct a brute-force or credential stuffing attack. We look at failed logins, successful logins after failures, source IPs, and timestamps to build a complete picture of the incident. The exact answers are derived from the live Elastic instance in the lab — use the queries below as your starting point.

## Walkthrough

**Step 1 — Open Kibana Discover**

Log into the Kibana instance. Navigate to **Discover** and select the relevant index pattern (e.g., `*` or the Windows/auth-specific index).

**Step 2 — Set the time range**

Adjust the time picker to cover the incident window specified in the room.

**Step 3 — Filter for authentication failures**

```kql
event.category: "authentication" and event.outcome: "failure"
```

Review the top source IPs. A single IP making hundreds of failures is a strong brute-force indicator.

**Step 4 — Filter for successful authentications**

```kql
event.category: "authentication" and event.outcome: "success"
```

Compare against the failure list — if a success from the same IP follows a burst of failures, that's the compromise event.

**Step 5 — Pivot on user accounts**

```kql
event.category: "authentication" and user.name: "TARGET_USER"
```

Expand individual log entries to review full field values — source IP, destination, timestamp, and authentication method.

**Step 6 — Correlate and document findings**

Use the Kibana dashboard provided in the room to locate exact answer values. The answers are dynamically generated per lab instance.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| *Room-specific questions* | *Derived from the Elastic lab instance provided in the room* |
