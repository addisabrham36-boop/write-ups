# SOC Role in Blue Team: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/socroleinblueteam

## Room Overview & Information

**Category:** Blue-team operations and cybersecurity careers  
**Objective:** Understand SOC responsibilities, analyst tiers, alert triage, escalation, and coordination with other security teams.

This is a defensive learning room rather than an exploitation target. Network scans, initial access, and privilege escalation are not applicable. The relevant workflow is an incident investigation from alert intake to evidence-based triage, escalation, response, and lessons learned.

## Phase 1: Reconnaissance & Enumeration of the Alert

### 1. Identify the event and affected asset

A SOC receives telemetry from systems such as SIEM platforms, endpoint detection tools, email gateways, firewalls, identity providers, and cloud audit systems. Before deciding that an alert is malicious, identify the rule that fired, timestamp, affected user/host, source and destination, and relevant evidence fields.

### 2. Gather context

A hospital may prioritise patient safety and availability; a bank may prioritise transaction integrity and confidentiality. Investigators need to understand the role of the asset and the potential business impact. Compare the event with normal activity, related alerts, and other data sources.

## Phase 2: Alert Triage & Initial Investigation

### 3. Follow a repeatable triage sequence

1. Read the alert title, detection logic, severity, and timestamp.
2. Identify the affected account, device, process, source, and destination.
3. Inspect the raw event or events that triggered the rule.
4. Search for related activity before and after the alert.
5. Compare the behaviour with known good activity and the asset's role.
6. Classify it as benign, suspicious, or confirmed malicious based on evidence.
7. Follow the playbook and escalate when the impact or uncertainty exceeds your authority.
8. Record the evidence, reasoning, and action in the case.

A detection is a signal, not a conclusion. An unusual login can be legitimate travel, VPN usage, or compromised credentials. One event rarely tells the whole story.

## Phase 3: Response, Escalation & Post-Incident Review

### 4. Understand SOC responsibilities and tiers

- **Tier 1 analyst:** monitors alerts, performs initial triage, gathers context, and escalates.
- **Tier 2 analyst:** investigates more deeply, correlates evidence, and supports containment.
- **Tier 3 analyst or threat hunter:** investigates complex activity, hunts for missed threats, and improves detections.
- **Incident response:** coordinates containment, eradication, recovery, and lessons learned.
- **Threat intelligence:** researches adversary techniques, campaigns, and indicators.
- **Detection engineering:** builds, tests, and tunes rules.
- **Security engineering:** designs and operates controls such as EDR, identity, firewalls, and SIEM pipelines.
- **SOC manager/security leader:** owns operations, prioritisation, metrics, and risk communication.

These are common patterns rather than a universal organisation chart. Smaller teams may combine responsibilities.

### 5. Escalate and contain safely

Escalate when an incident involves a critical asset, successful unauthorised access, suspected data exfiltration, ransomware, privileged-account compromise, or a need for specialist skills. A good handoff includes a summary, timeline, affected systems, supporting evidence, actions already taken, and the questions that remain.

Isolating a host, disabling an account, or blocking network traffic may disrupt operations. Follow the organisation's response authority and preserve evidence where practical. Do not exceed your role just to close an alert quickly.

### 6. Measure and improve

Useful operational measurements include time to detect, time to triage, time to contain, alert quality, and coverage of critical assets. Metrics require context: fewer alerts may mean better tuning or poor detection coverage. Review false positives, missed incidents, and telemetry gaps and test changes against benign and suspicious examples.

## Phase 4: Room Tasks & Answer Method

The room's exact interactive questions may change. For each task, identify whether it concerns the SOC mission, analyst responsibilities, tier roles, escalation, or career progression. Use the official task wording and options; do not invent a quiz answer that has not been observed in the room.

For a scenario question, the safest general method is to validate raw telemetry, gather context, document the evidence, and escalate according to impact and playbook. Do not label an event confirmed malicious from a severity score alone.

## Phase 5: Key Takeaways & Remediation

A SOC is a coordinated function in the broader Blue Team. Useful operations require reliable telemetry, alert validation, contextual investigations, clear escalation paths, documented playbooks, and ongoing detection improvement. Analysts should distinguish facts from assumptions and communicate uncertainty plainly.

A portfolio investigation should explain the question, evidence, timeline, decision, and remediation while removing credentials, customer data, and other sensitive information.