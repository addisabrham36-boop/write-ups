# SOC Role in Blue Team: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/socroleinblueteam

## Room Overview & Information

**Category:** Blue-team operations and careers  
**Focus:** SOC responsibilities, analyst tiers, escalation, and career development.

A Blue Team protects an organisation by preventing, detecting, investigating, and responding to security incidents. A Security Operations Centre (SOC) monitors security telemetry and coordinates investigations, but it works with system owners, IT operations, identity teams, incident responders, threat intelligence, and security engineering.

## Task 1: Introduction

The SOC is not an isolated team that owns every security task. Analysts work with other teams to understand the business impact of an alert and to ensure that containment does not create unnecessary disruption.

A hospital may prioritise patient safety and availability, a law firm may prioritise confidentiality, and a bank may prioritise transaction integrity as well as confidentiality. Business context changes how an incident is prioritised.

## Task 2: Common SOC roles

- **CISO/security leader:** owns security strategy, risk posture, and executive reporting.
- **SOC manager or lead:** coordinates monitoring operations, staffing, and process quality.
- **Tier 1 analyst:** monitors alerts, performs initial triage, gathers context, and escalates.
- **Tier 2 analyst:** conducts deeper investigations and supports containment.
- **Tier 3 analyst or threat hunter:** investigates complex incidents, develops detections, and searches for activity that automated alerts may miss.
- **Incident response team:** coordinates containment, eradication, recovery, and lessons learned.
- **Threat intelligence:** researches adversaries, campaigns, and indicators.
- **Detection engineering:** builds, tests, and tunes detection rules.
- **Security engineering:** designs and operates controls such as EDR, identity, firewalls, and SIEM pipelines.

These are common patterns, not a universal org chart. Smaller organisations may combine several responsibilities in one role.

## Task 3: How a Tier 1 analyst investigates

A Tier 1 analyst often receives an alert from a SIEM, endpoint tool, email gateway, or network sensor. A repeatable triage sequence is:

1. Read the alert name, severity, detection rule, and timestamp.
2. Identify the affected user, host, source, and destination.
3. Inspect the raw event or events that caused the alert.
4. Search for related activity before and after the event.
5. Compare the activity with the user's normal behaviour and the asset's role.
6. Decide whether the event is benign, suspicious, or confirmed malicious.
7. Follow the playbook and escalate when impact or uncertainty exceeds the analyst's authority.
8. Record the evidence, decisions, and actions in the case.

A detection is a signal, not a final conclusion. A successful login from an unusual location could be legitimate travel, VPN use, or compromised credentials. Context and corroborating evidence are needed.

## Task 4: Escalation and communication

Escalate when an incident involves a critical asset, successful unauthorised access, suspected data exfiltration, ransomware, privileged-account compromise, or activity requiring specialist skills. Include a summary, timeline, affected systems, supporting evidence, actions already taken, and specific questions for the next team.

Good communication matters because an incident can affect service availability and business operations. Distinguish confirmed facts from hypotheses and do not overstate the evidence.

## Task 5: Career progression

SOC analysts may progress toward incident response, threat hunting, detection engineering, threat intelligence, security engineering, cloud security, or management. Useful foundations include networking, Windows and Linux administration, identity systems, log analysis, SIEM queries, endpoint telemetry, scripting, and written communication.

Keep a portfolio of sanitised investigations that shows the question, evidence, reasoning, conclusion, and remediation. This demonstrates judgement more clearly than a list of tools alone.

## Working through the room

For each task, identify whether it concerns organisational roles, analyst workflow, escalation, or career development. Use the scenario and task wording to choose the answer. Do not assume every organisation uses identical job titles.

## Key takeaways

The SOC is a coordinated function within the broader Blue Team. Analysts validate alerts, gather context, and escalate appropriately; specialised teams investigate complex activity, build detections, and improve controls. Strong operations depend on useful telemetry, documented playbooks, and clear communication.

## Expanded SOC operating workflow

### 1. Understand the SOC mission

A Security Operations Center monitors security telemetry, investigates alerts, coordinates response, and improves detections. Roles and escalation rules vary. Analysts should know what they may change themselves and which actions require an incident commander or system owner.

### 2. Triage alerts

Start with the alert source, rule logic, timestamp, severity, asset, user, and evidence fields. Validate the event in raw telemetry before deciding whether it is malicious. Correlate authentication, endpoint, DNS, network, and cloud events to build context.

### 3. Investigate and build a timeline

Normalize timestamps, track affected assets and identities, and look for a sequence of actions rather than one isolated indicator. Consider benign explanations and document why they are accepted or rejected. Preserve the query and evidence so another analyst can reproduce the finding.

### 4. Escalation and containment

Escalate according to severity, uncertainty, and potential impact. Host isolation, account disablement, or network blocking can interrupt business; follow the response plan and preserve evidence where practical. Do not exceed authorization merely to close an alert quickly.

### 5. Detection engineering and feedback

When an alert is a false positive, identify which condition caused the noise. When an incident is missed, determine what telemetry or rule was absent. Improve the detection without overfitting to a single sample and test against benign and suspicious cases.

### 6. Operational metrics

Time to detect, time to triage, and time to contain can help measure operations, but metrics need context. A low alert count may reflect strong security or poor coverage; a high count may reflect increased activity or noisy rules. Measure outcomes, not just volume.

### 7. Room questions

For each question, identify the role or process being tested and answer from the actual room scenario. Distinguish monitoring, investigation, response, and threat hunting. Record supporting evidence and redact flags as flag{hidden}.
