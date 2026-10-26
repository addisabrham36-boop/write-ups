# Junior Security Analyst Intro

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

## Phase 5: Final Question and Evidence Checklist

| Exact question | Answer or evidence |
|---|---|
| What will be your role as a Junior Security Analyst? | Triage Specialist |
| What was the malicious IP address in the alerts? | 221.181.185.159 |
| To whom did you escalate the event associated with the malicious IP address? | Will Griffin |
| After blocking the malicious IP address on the firewall, what message did the malicious actor leave for you? | Copy the flag returned by the active dashboard; it is redacted here as THM{hidden} |

Before submitting, confirm that the malicious IP, escalation contact, and post-blocking message match the current dashboard. The first three answers are the documented room values; the flag must be read after the simulated firewall action succeeds.

## Remediation and SOC practice

A production analyst should preserve the alert and supporting logs, validate the IP across multiple events, escalate with a timeline and evidence, and apply blocks only under the approved playbook. After containment, confirm that the rule is active, monitor for repeat attempts or alternate sources, and document the residual risk. Blocking one address does not remove malware or revoke stolen credentials.
