# Junior Security Analyst Intro: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/securityanalystintroda

## Room Overview & Information

**Category:** SOC / defensive security  
**Difficulty:** Beginner  
**Focus:** The daily duties of a Level 1 security analyst, alert triage, incident investigation, collaboration, and continuous learning.

A Junior Security Analyst is often a first-line triage analyst in a Security Operations Centre (SOC). The job is not simply to click every alert or label every event malicious. It involves reviewing evidence, prioritising risk, checking context, documenting decisions, and escalating incidents that need deeper investigation.

## Task 1: Junior Security Analyst Journey

The room frames the analyst's day around current security news and the evolving threat landscape. News can provide context about campaigns, vulnerabilities, ransomware, supply-chain compromise, and data breaches. However, a news article is not a substitute for evidence from the organisation's own environment.

### Daily responsibilities

A Level 1 analyst commonly:
- Monitors and triages security alerts.
- Checks whether activity matches known normal behaviour.
- Collects relevant host, identity, and network evidence.
- Escalates suspicious events according to a runbook.
- Communicates clearly with other security and IT teams.
- Documents actions and maintains an accurate case timeline.
- Continues learning about new attack techniques and defensive controls.

## How to approach the room's analyst scenarios

For each interactive task, identify the alert or evidence source the question names. Read the actual event details before selecting an answer. In a real SIEM, the fields commonly worth checking include event timestamp, user or account, source and destination IPs, hostname, process, event type, action, and outcome.

Use this triage sequence:

1. **Validate the signal.** Confirm that the alert is based on an actual event and identify the detection rule that fired.
2. **Add context.** Check whether the account, device, source address, and time are expected.
3. **Scope the activity.** Search for related events from the same identity, host, process, or source.
4. **Assess impact.** Determine whether access succeeded and whether sensitive resources were reached.
5. **Contain or escalate.** Follow the organisation's runbook and authority limits.
6. **Document.** Record the evidence, decision, and next action.

## Useful investigation concepts

### False positive and false negative

A false positive is benign activity incorrectly flagged as suspicious. A false negative is malicious activity that was not detected. Analysts must reduce both, but they should not dismiss a high-risk alert simply because some alerts are noisy.

### Indicator versus conclusion

An IP address, domain, file hash, or username is an indicator. The conclusion that an incident occurred requires context and corroborating evidence. For example, a connection to a suspicious IP may be blocked before it succeeds; that is different from proving data was stolen.

### Escalation

Escalate when the event exceeds the analyst's authority, requires specialist expertise, involves high-impact assets, or has evidence of successful compromise. Include the timeline, affected identities and hosts, relevant log excerpts, actions already taken, and unanswered questions.

## Working through the interactive tasks

The exact values in the room's dashboard or scenarios are environment-specific. For each question:
1. Open the relevant dashboard or scenario from the room.
2. Read the alert and expand its details.
3. Locate the field requested by the question.
4. Compare the value with the surrounding context.
5. Submit the value shown by the current room.
6. Record the reasoning without publishing sensitive operational data.

Do not use a random IP, username, or flag from another learner's write-up. If the task asks for a specific event field, the authoritative answer is the evidence displayed by the active room.

## Key takeaways

A junior analyst adds value by being methodical. Validate the alert, correlate evidence, assess impact, follow the playbook, and communicate the result. Speed matters in a SOC, but an unsupported conclusion can cause as much trouble as a missed alert.