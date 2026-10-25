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

## Expanded SIEM workflow

### 1. What a SIEM does

A Security Information and Event Management platform collects security-relevant events from multiple sources, parses and normalizes them, correlates related activity, and supports searching, alerting, dashboards, and investigation. It does not automatically understand every event; data quality and rule quality determine what analysts can see.

Common sources include Windows event logs, Linux authentication logs, DNS, firewalls, proxies, cloud audit logs, identity providers, and endpoint detection platforms. Record the source and field semantics before writing a query.

### 2. Confirm the data source and time window

Start with the task's stated index, table, dataset, or log source. Confirm the relevant time range and timezone. A query returning no events may mean the event did not occur, the data was not ingested, the index is wrong, or the time filter excludes it.

A good investigation starts broad enough to find the event, then narrows based on evidence. Avoid guessing field names without checking the schema or the room's interface.

### 3. Search and filter events

Use the search language supported by the lab's SIEM. In any query, understand the difference between exact matches, wildcard matches, Boolean operators, and time filters. Preserve the query used to obtain an answer so the result can be reproduced.

For each result, inspect timestamp, host, user, source/destination address, event type, process or application, and outcome where available. Do not assume that a field has the same meaning across all data sources.

### 4. Correlate related events

A single failed login can be ordinary user error. Many failures followed by a success, especially from a new source or against multiple accounts, may indicate password spraying or brute force. A new process followed by an unusual outbound connection can suggest malicious execution, but corroborate it with endpoint or network evidence.

Build a timeline and look for sequence, frequency, rarity, and context. Check whether the host is expected to perform the activity and whether the account normally uses that source.

### 5. Detection rules and false positives

A detection rule should identify a meaningful behaviour and include enough context for triage. Too broad a rule creates noise; too narrow a rule misses variants. Test the rule against known benign and suspicious data, and document its assumptions.

Severity should reflect the potential impact and confidence, not simply the number of events. A high-severity alert still needs investigation, and a low-severity event can become important when correlated with other evidence.

### 6. Triage and incident response

Validate the alert, identify affected assets and accounts, determine the likely attack stage, preserve evidence, and escalate according to procedure. Containment actions such as disabling an account or isolating a host can disrupt business, so use the organization's authority and process. In a training room, follow the task instructions and do not alter unrelated systems.

### 7. Answer the room questions

Find the relevant event in the stated source and time range. Verify the requested field directly in the event record rather than inferring it from a dashboard summary. Preserve exact formatting for IP addresses, usernames, event IDs, and timestamps. Retrieve flags from the active room and redact them as `flag{hidden}` in public notes.

### 8. Operational recommendations

Normalize timestamps, maintain reliable ingestion, protect log integrity, restrict access to sensitive records, monitor pipeline health, and review detections regularly. A SIEM is only as useful as the data it receives and the quality of the investigations built around it.
