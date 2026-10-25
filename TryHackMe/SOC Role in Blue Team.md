# SOC Role in the Blue Team: TryHackMe Walkthrough

Room: https://tryhackme.com/room/socroleinblueteam

A Security Operations Centre monitors an organisation's systems and investigates signs of compromise. The SOC is part of the blue team: its job is to protect the organisation, detect suspicious behaviour, coordinate response, and improve detection over time.

## 1. What a SOC does

A SOC commonly performs continuous monitoring, alert triage, threat hunting, incident escalation, and reporting. It may work with incident responders, IT operations, threat intelligence teams, detection engineers, and business owners.

A tiered model is common but not universal:
- **Tier 1:** reviews incoming alerts, gathers context, and follows triage procedures.
- **Tier 2:** performs deeper investigation, correlates evidence, and handles escalated cases.
- **Tier 3 or specialist teams:** investigate complex incidents, conduct threat hunting, reverse engineer malware, or improve detection content.

Titles and responsibilities differ by organisation. What matters is that alerts have a clear owner and a defined escalation path.

## 2. Triage an alert

A disciplined analyst does not immediately assume the alert is malicious or harmless. Start with the detection name, timestamp, affected asset, identity, source, and triggering evidence.

Then:
1. Inspect the raw event and understand why the rule fired.
2. Identify the user and device involved.
3. Search related events around the same time.
4. Compare the behaviour with normal activity or approved changes.
5. Assess the potential impact and confidence.
6. Record the reasoning and escalate when required.

The quality of the investigation depends on evidence. A suspicious IP match may be useful, but context such as the destination service, user activity, and sequence of events is needed to understand the risk.

## 3. Use threat intelligence carefully

Threat intelligence can provide information about malicious domains, IP addresses, file hashes, tactics, techniques, and known campaigns. Indicators become less useful when they are old, shared, or easy to change. Use intelligence to guide investigation, not as the only reason to declare an incident.

Map observed behaviour to a framework such as MITRE ATT&CK when it helps communicate the technique. A technique label should be supported by the observed activity, not added merely because it sounds relevant.

## 4. Incident handling

When evidence suggests a real incident, follow the organisation's response plan. Typical stages include preparation, identification, containment, eradication, recovery, and lessons learned. The exact order can vary, and containment may need to happen before every detail is known if the risk is severe.

Preserve evidence before taking actions that may destroy it, unless immediate containment is necessary to prevent greater harm. Coordinate with the responsible team and document changes, approvals, and results.

## 5. Measure and improve

SOC metrics may include time to triage, time to detect, time to contain, alert volume, false-positive rate, and coverage of critical assets. Metrics need context: reducing alert volume is not a success if important detections are also being lost.

Detection engineering should use feedback from investigations. Repeated false positives may indicate that a rule needs tuning, while an incident with no alert may reveal a visibility gap.

## 6. Working through the room

For each scenario, identify the SOC activity being described: monitoring, triage, investigation, threat hunting, escalation, or incident response. If the task presents an alert, use the evidence in that alert and the surrounding context. Do not classify an event solely by a single indicator.

## What I took from this room

A SOC is not just a wall of dashboards. It is a repeatable process for turning telemetry into evidence-based decisions, escalating real threats, and feeding lessons back into security controls.