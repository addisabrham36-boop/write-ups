# Introduction to SIEM: TryHackMe Walkthrough

Room: https://tryhackme.com/room/introtosiem

A Security Information and Event Management system collects logs from different sources, normalises them, and helps analysts search for suspicious activity. It provides a shared view of events that would otherwise be scattered across endpoints, servers, identity providers, firewalls, and applications.

## 1. Understand event sources

Common SIEM sources include:

- Authentication logs: logins, failures, MFA events, and account changes.
- Endpoint logs: process creation, file activity, and security detections.
- Network logs: DNS queries, connections, firewall decisions, and proxy requests.
- Cloud audit logs: changes to resources, permissions, and service configurations.
- Application logs: requests, errors, and access to sensitive functions.

Each source has blind spots. A firewall log may show a connection but not what a user did inside an application. An endpoint log may show a process but not every network event. Correlation is useful because it connects these partial views.

## 2. Know the important fields

A log event commonly includes a timestamp, source, event type, user or process, host, source and destination addresses, and a message or result. Field names differ by product. Before writing a query, inspect the available fields and a few representative events.

Timestamps require care. Confirm the timezone and whether the platform has normalised event time or ingestion time. A late-arriving event may appear in the SIEM after the activity actually happened.

## 3. Search and filter

Start with a narrow time range and one useful field. For example, search for failed authentication events for a particular account, then expand the window to find successful logins, new devices, or privilege changes around the same time.

The exact query syntax depends on the SIEM. Do not copy syntax from Splunk, Elastic, Sentinel, or another platform without checking which one the room uses. The general approach is to filter by time, event type, user, host, and relevant indicator, then compare related records.

## 4. Correlate events into a timeline

One event rarely tells the whole story. A suspicious login may be followed by MFA changes, a new process, access to sensitive files, or unusual outbound connections. Correlation links those events into a possible attack chain.

For each event, ask:
1. Is the timestamp consistent with the other records?
2. Is the source host or identity expected?
3. Does the event have a related parent or follow-on action?
4. Is the behaviour common for this user or host?
5. What additional evidence would confirm or refute the hypothesis?

Avoid assuming that correlation proves causation. The timeline supports an interpretation, but alternative explanations should be considered.

## 5. Indicators, detections, and false positives

An IOC can be an IP address, domain, hash, filename, or other observable clue. Detection rules combine indicators and behaviour into alerts. A match is a reason to investigate, not proof by itself. Shared hosting, legitimate administrative activity, software updates, and testing tools can produce events that look suspicious.

A good detection rule should be tested against known malicious and benign examples. Analysts should document why an alert was closed or escalated and provide feedback when a rule is too noisy or misses relevant activity.

## 6. Practical triage steps

1. Open the alert and identify the rule that fired.
2. Set the relevant time window and inspect the raw event.
3. Identify the affected identity, endpoint, and network locations.
4. Search related events before and after the alert.
5. Compare the activity with a known baseline or approved change.
6. Decide whether to close, monitor, or escalate according to policy.
7. Preserve the query and evidence so another analyst can reproduce the conclusion.

## 7. Working through the room

Use the platform and dataset supplied by the room. Read the task question carefully, identify which field or event type it refers to, and narrow the search before interpreting the result. If a question asks for a value from a log, copy the value from the displayed record rather than inventing an example.

## What I took from this room

A SIEM is useful because it brings events together, but the analyst still has to ask the right questions. Reliable triage depends on understanding fields, timestamps, context, and the limits of each log source.