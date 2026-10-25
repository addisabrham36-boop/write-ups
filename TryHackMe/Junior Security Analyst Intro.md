# Junior Security Analyst Intro: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/jrsecanalystintrouxo

## Room Overview & Information

**Category:** SOC / defensive security  
**Difficulty:** Easy  
**Scenario:** Play through a junior analyst's shift, triage alerts, identify a malicious IP address, escalate the event, and block the source.

A Junior Security Analyst is often a first-line triage analyst in a Security Operations Centre. The job is not simply to click every alert or label every event malicious. It involves reviewing evidence, prioritising risk, checking context, documenting decisions, and escalating incidents that need deeper investigation.

## Task 1: A career as a Junior Security Analyst

The room introduces the responsibilities of an entry-level analyst. A junior analyst commonly monitors alerts, checks event context, gathers evidence, follows a playbook, and escalates suspicious activity when deeper investigation is required.

### Question: “What will be your role as a Junior Security Analyst?”

**Answer: Triage Specialist.**

Triage means reviewing incoming alerts, deciding which require immediate attention, and collecting enough evidence to route the event to the correct team. A triage specialist should distinguish an alert from a confirmed incident and record the reasoning behind the decision.

## Task 2: Investigate the Alerts

Open the room's dashboard using the **View Site** control. Review the alerts and their event details. Look for the source address associated with malicious activity and compare the events around it.

When investigating an alert, check:
- Timestamp and timezone.
- Source and destination IP addresses.
- User or account involved.
- Hostname and service.
- Action and result.
- Related events before and after the alert.

### Question: “What was the malicious IP address in the alerts?”

**Room answer:** `221.181.185.159`.

Verify that this address matches the malicious activity shown in the active dashboard. In a real investigation, an IP address is an indicator, not proof of a person's identity; shared hosting, VPNs, and compromised devices can complicate attribution.

## Task 3: Escalate the Event

Escalation ensures that a suspicious event reaches the person or team responsible for deeper investigation. A good escalation includes the alert, relevant timestamps, source and destination, affected system, evidence collected, actions already taken, and the specific question that needs follow-up.

### Question: “To whom did you escalate the event associated with the malicious IP address?”

**Room answer:** **Will Griffin**.

The room's dashboard identifies the escalation contact. In a real SOC, follow the organisation's escalation matrix rather than selecting a contact based on guesswork.

## Task 4: Block the Malicious IP

The lab asks you to block the malicious source address using the simulated firewall. Open the firewall controls, add the address shown in the alert, select the block action, and apply the rule. Wait for the dashboard to confirm the change.

Blocking an IP is a containment action. It can reduce ongoing traffic from that source, but it does not remove malware, revoke stolen credentials, or prove the attacker has no alternate address. In a real environment, assess collateral impact and monitor for activity from other sources.

### Question: “After blocking the malicious IP address on the firewall, what message did the malicious actor leave for you?”

The dashboard displays a flag after the rule is applied. Submit the value from the active room. This public write-up redacts it as `THM{hidden}`.

## Investigation checklist

A junior analyst should:
1. Validate the alert and identify the event that triggered it.
2. Check the source, destination, time, and affected asset.
3. Search for related events.
4. Decide whether the activity is suspicious and document why.
5. Escalate to the correct contact with supporting evidence.
6. Apply only the containment actions authorised by the playbook.
7. Verify that the action was accepted and continue monitoring.
8. Record the timeline and remaining uncertainty.

A successful firewall-rule update is evidence that the configuration change was accepted. In a production incident, verify its effect using firewall logs and subsequent traffic.

## Key takeaways

The room follows a simple SOC workflow: triage, identify the malicious source, escalate the event, and contain the activity. The correct answers come from the dashboard: **Triage Specialist**, **221.181.185.159**, and **Will Griffin**. The final flag is obtained after the simulated firewall block and is redacted here for the public repository.

## Expanded SOC analyst workflow

### 1. Define the investigation question

A junior analyst does not start by declaring an incident. Start with the alert, its source, the time range, the affected asset, and the detection logic that fired. Identify the evidence needed to confirm or dismiss the alert, and record the limits of the available telemetry.

### 2. Triage the alert

Review the alert's timestamp, hostname, user, source and destination addresses, process or application, detection name, severity, and any linked events. A severity label is a prioritization hint, not a conclusion. Correlate the event with the asset's role, the user's normal activity, and nearby events.

### 3. Search related telemetry

If the room supplies a SIEM, search around the alert time and expand the time window only when evidence justifies it. Useful sources may include authentication events, endpoint process creation, DNS queries, proxy records, firewall logs, and cloud audit logs. Preserve the query and fields used so another analyst can reproduce the result.

### 4. Distinguish true and false positives

A true positive means the detection identified behaviour that matches the intended suspicious condition; it does not always mean a confirmed compromise. A false positive is benign activity incorrectly flagged. A true negative is benign activity correctly not alerted on; a false negative is malicious activity missed by the detection.

Document why the event fits or does not fit the detection. Do not close an alert merely because the affected user is familiar or because one source appears normal.

### 5. Containment and escalation

Follow the room's or organization's response procedure. Preserve relevant evidence before making changes where practical. Isolating a host, disabling an account, or blocking an address can affect business operations and should follow the authorized process. Escalate promptly when impact, uncertainty, or scope exceeds the analyst's role.

### 6. Firewall block and outcome validation

If the task asks for a simulated firewall block, identify the source, destination, protocol, and rule context. Confirm what the simulated control changed and whether subsequent telemetry reflects the expected effect. A successful policy update does not by itself prove the suspicious activity has stopped.

### 7. Write a useful case note

A case note should include the alert summary, evidence sources, timeline, findings, confidence, actions taken, and next steps. Separate observed facts from assumptions. Avoid copying secrets, full personal data, or public flags into a portfolio.

### 8. Room questions

Answer each task from the event records and controls shown in the active room. Use exact values when a question asks for a host, account, rule, or event field. Do not fabricate a detection or claim containment succeeded without evidence; redact public flags as `flag{hidden}`.
