# Introduction to SIEM — TryHackMe

Room: https://tryhackme.com/room/introtosiem

A SIEM, or Security Information and Event Management system, collects security-related events from different systems and helps analysts search, correlate, and investigate them. Instead of opening logs on every server separately, the analyst can use one place to look for patterns.

## What a SIEM collects

Sources can include firewalls, servers, endpoint protection, identity systems, cloud services, and applications. The data may contain login attempts, process activity, network connections, alerts, or configuration changes. Collection alone is not enough: logs need useful timestamps, fields, and context to be searchable.

## Events, alerts, and correlation

An event is a recorded occurrence. An alert is usually generated when a rule or analytic identifies activity that deserves attention. A correlation rule can connect events that look ordinary on their own but become suspicious together, such as repeated failed logins followed by a successful login from an unusual source.

An alert is not proof of an attack. I need to investigate the underlying events, identify the affected account or host, check the timeline, and look for supporting evidence. Otherwise, the SOC becomes a machine for producing noise, which is not the career goal.

## A basic investigation workflow

1. Read the alert and identify the rule that triggered it.
2. Note the relevant user, host, IP address, and time range.
3. Search related events before and after the alert.
4. Compare the activity with normal behaviour and known threat indicators.
5. Record evidence, reasoning, and the next action.

The exact query language depends on the SIEM. I should use the syntax documented for the platform rather than assume every product uses the same search commands.

## What I took from this room

A SIEM gives security teams a central view of activity, but it is only as useful as the data and detection logic behind it. Analysts still need to reason about alerts, investigate context, and document decisions. The goal is to turn large amounts of log data into a clear understanding of what happened.