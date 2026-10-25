# Junior Security Analyst Intro: TryHackMe Walkthrough

Room: https://tryhackme.com/room/jrsecanalystintrouxo

A junior security analyst often begins with alert triage: determine what triggered an alert, gather context, decide whether it is benign or suspicious, and escalate according to procedure. The role requires careful reasoning and clear notes, not just knowing tool names.

## 1. Understand the SOC workflow

A Security Operations Centre receives telemetry from endpoints, identity systems, network devices, cloud platforms, and applications. Detection rules use that telemetry to generate alerts. Analysts investigate alerts and create cases when the evidence suggests a genuine threat.

A common workflow is:
1. Read the alert and identify its detection logic.
2. Confirm the time, host, user, and indicator.
3. Inspect the raw event.
4. Search for related events.
5. Compare the behaviour with normal activity.
6. Decide whether to close, monitor, or escalate.
7. Record evidence and reasoning.

## 2. Identify common evidence sources

Authentication logs can show successful and failed logins, MFA events, and account changes. Endpoint telemetry can show process execution, file activity, and security detections. DNS, firewall, and proxy logs can show attempted communication. Cloud audit logs can show permission or resource changes.

Each source has limits. A network event may show a connection but not the data exchanged. A process event may show an executable but not whether it was malicious. Correlate multiple sources before making a strong claim.

## 3. Use indicators carefully

Indicators of compromise include suspicious domains, IP addresses, hashes, filenames, or behaviours. An indicator can be shared by legitimate and malicious activity, and attackers can change infrastructure. Threat intelligence provides context but should not replace local evidence.

When an alert includes an indicator, check its age, source, confidence, and relevance to the observed event. Record whether the indicator was directly observed or only returned by a third-party source.

## 4. Prioritise an alert

Consider the asset's importance, the privileges of the affected identity, whether the activity succeeded, the sensitivity of accessed data, and whether the behaviour is ongoing. A failed login from an unfamiliar IP and a successful privileged login followed by suspicious file access should not receive identical treatment.

Use the organisation's severity and escalation criteria. Do not lower severity merely because the alert is unfamiliar, and do not declare an incident based on a single weak signal.

## 5. Write a useful case note

A case note should state what triggered the alert, what evidence was reviewed, what the analyst concluded, and what happens next. Include relevant timestamps, host and user identifiers, event references, and queries used. Avoid unsupported claims and unnecessary sensitive data.

## 6. Working through the room

Follow the scenario and identify the analyst's next best action. If the task presents logs, read the actual event fields and compare related records. If it asks about a role or tool, match the responsibility to the task: triage is different from containment, forensics, threat hunting, and detection engineering.

## What I took from this room

Good SOC work is evidence-driven. A junior analyst should know how to read an alert, gather context, recognise uncertainty, escalate correctly, and document the reasoning so another analyst can continue the investigation.