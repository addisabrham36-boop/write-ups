# Introduction to SIEM: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/introtosiem

## Room Overview & Information

**Category:** Defensive security / SOC  
**Difficulty:** Beginner  
**Focus:** Log sources, centralisation, parsing and normalisation, correlation, detection rules, alert investigation, and analyst decisions.

A Security Information and Event Management (SIEM) platform collects logs from different systems, normalises their fields, correlates related events, and helps analysts detect and investigate suspicious behaviour. This room includes a static lab dashboard. Values such as process names, usernames, and hostnames must be read from the active dashboard rather than guessed.

## Task 1: Introduction

**Question: “What does SIEM stand for?”**

Answer: **Security Information and Event Management**.

SIEM platforms are used by SOC analysts to investigate activity across endpoints, servers, network devices, identity systems, cloud services, and applications. A SIEM is only as useful as its log coverage, parsing, detection logic, retention, and analyst workflow.

## Task 2: Logs Everywhere, Answers Nowhere

Different devices generate different logs. A firewall records network decisions, a Windows endpoint records operating-system events, a Linux server records authentication and service events, and a web application may record HTTP requests. Each source provides only part of the story.

Analysing each system separately creates several problems:
- There may be too many events for manual review.
- Logs use different formats and field names.
- Investigators lose time connecting to individual hosts.
- An isolated event may look normal while a sequence of related events is suspicious.
- Retention may differ between systems, leaving gaps in the timeline.

Centralisation gives analysts a searchable timeline across these sources.

## Task 3: Why SIEM?

A SIEM typically provides:
- **Collection:** receives logs through agents, APIs, collectors, or network integrations.
- **Parsing:** extracts fields from raw records.
- **Normalisation:** maps different sources into a consistent schema.
- **Correlation:** links events based on shared fields or time.
- **Detection:** evaluates rules and analytics against event data.
- **Alerting:** notifies analysts when a rule matches.
- **Dashboards and reporting:** summarises event volume, alerts, trends, and system health.
- **Retention and search:** supports investigations and compliance needs.

Example detection logic: if a user has five failed logins in ten seconds and then a successful login, generate an alert for possible password guessing. The rule is a hypothesis about suspicious behaviour, not proof that an attacker succeeded.

## Task 4: Log Sources and Ingestion

Windows logs are available through Event Viewer and can be forwarded to the SIEM. Windows event IDs identify categories of events; for example, event ID 4688 can record process creation when the relevant audit policy is enabled. Event ID 104 can indicate an event log was cleared in the Windows Event Log service context.

Linux systems commonly use systemd journal and files under `/var/log`, depending on the distribution and service. Useful inspection commands on a system you administer include:

```bash
journalctl --since "1 hour ago"
journalctl -u sshd
sudo tail -n 100 /var/log/auth.log
```

The exact log path varies by distribution. These are examples for learning and administration, not commands that must be run against the room's static dashboard.

Ingestion must preserve timestamps and parse important fields correctly. If the source timezone is wrong or the user field is parsed incorrectly, correlation rules can produce misleading results.

## Task 5: Alerting Process and Analysis

A detection rule checks event fields and conditions. Examples include:
- A high number of failed logins from one source.
- A successful login shortly after repeated failures.
- A process that clears event logs.
- A suspicious executable launched by a web server.
- Unexpected outbound traffic from a sensitive host.

The room describes an event-log-clearing rule using source `WinEventLog` and event ID `104`. It also describes a process-creation rule using event ID `4688` and a process name containing `whoami`. These are examples of detection logic; their effectiveness depends on logging configuration, field parsing, and the environment's baseline.

A good analyst checks which conditions matched, reviews the raw event, searches for related activity, and decides whether the alert is a true positive or false positive. A true positive means the detection correctly identified activity that meets the rule's suspicious or malicious condition. A false positive means the alert fired on benign activity. A true positive is not always proof of a successful compromise; it confirms the activity represented by the alert.

## Task 6: Lab Work

Open the room's **View Site** dashboard. Click **Start Suspicious Activity** and inspect the alert and associated event data.

### Question 1: “After clicking on the Start Suspicious Activity button, which process caused the alert?”

1. Trigger the activity in the dashboard.
2. Open the resulting alert.
3. Inspect the event or process details linked to the alert.
4. Read the process name that matched the detection.
5. Submit the exact process name shown by the lab.

Do not infer the process from the alert title alone; confirm it in the event record.

### Question 2: “Find the event that caused the alert and identify the user responsible for the process execution.”

1. Open the event linked to the alert.
2. Locate the user/account field associated with the process event.
3. Confirm that you are reading the event that caused this alert, not a neighbouring event.
4. Submit the exact username.

### Question 3: “What is the hostname of the suspect user?”

Use the same process event and inspect its hostname or computer field. Do not confuse the username with the hostname. Copy the hostname exactly as displayed.

### Question 4: “Examine the rule and the suspicious process; which term matched the rule that caused the alert?”

Open the detection rule and inspect the condition that matched. Compare the process details with the rule's field and value. The answer is the term in the rule that actually matched the event. Do not guess based on the process name if the rule is matching a different field.

### Question 5: “Which option best represents the event? Choose from the following: False Positive / True Positive”

Compare the event with the rule's intended detection. If the suspicious activity was deliberately generated by the lab and matches the malicious behaviour the rule is designed to detect, select **True Positive**. If the event were benign but matched accidentally, it would be a false positive. Confirm using the lab's actual event details.

### Question 6: “Selecting the right ACTION will display the FLAG. What is the FLAG?”

Choose the action indicated by the task's evidence and submit it. The simulator displays the flag after the correct action. Obtain the flag from the live dashboard and redact it as `flag{hidden}` in a public write-up; do not invent a value.

## Phase 7: Investigation checklist

For each alert, record:
- Detection name and rule ID.
- Event timestamp and timezone.
- User, host, process, source, and destination.
- The exact rule condition that matched.
- Related events before and after the alert.
- True-positive or false-positive assessment and rationale.
- Containment or escalation decision.
- Remaining uncertainty and next steps.

## Phase 8: Key takeaways and remediation

A SIEM does not magically know which activity is malicious. It depends on telemetry, normalised fields, well-designed rules, and analyst validation. Improve detection by ensuring relevant logs are enabled, clocks are synchronised, fields are parsed consistently, and rules are tested against both malicious and benign cases. Keep a record of tuning changes and avoid discarding alerts simply because they are noisy.

## Phase 9: Complete the alert investigation

For the final interactive alert task, identify the relevant event and the rule that matched it before deciding what action the interface expects.

1. Open the room's supplied dashboard and select the task-specific alert.
2. Inspect event time, source, destination, user, process, and result fields.
3. Open the rule details and read the actual condition that matched.
4. Compare the observed activity with the rule's intent and the scenario.
5. Select True Positive only when the event matches the suspicious behaviour the rule is designed to detect. Use False Positive when the match is benign.
6. Choose the action requested by the active room and verify the dashboard's response.
7. Copy the displayed flag only after the action succeeds.

The event record and rule definition determine the answer. A suspicious process name alone is not enough to choose a match term, and a completed UI click is not proof that a control was applied successfully.

## Analyst checklist

Preserve the query, event identifier, timestamp and timezone, matched rule, evidence supporting the classification, chosen action, and validation result. In production, use a documented playbook, escalate when impact or uncertainty is high, and confirm containment through subsequent logs. Retune the detection if it misses the intended pattern or alerts on normal activity.
