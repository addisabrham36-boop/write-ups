# Defensive Security Intro: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/defensivesecurityintro

## Room Overview & Information

**Category:** Defensive security  
**Difficulty:** Easy  
**Focus:** Blue Team responsibilities, Security Operations Centres (SOCs), Digital Forensics and Incident Response (DFIR), and common malware threats.

Offensive security tests systems by simulating attacker behaviour. Defensive security focuses on protecting systems, detecting suspicious activity, investigating incidents, and reducing harm. This room introduces the people and processes that help organisations defend their networks.

## Task 1: Introduction to Defensive Security

Defensive security includes monitoring, threat intelligence, incident response, malware analysis, and security engineering. These functions work together: monitoring detects a possible event, investigation determines what happened, and response contains the threat and restores trusted operation.

### Question: “Which team focuses on defensive security?”

**Answer: Blue Team.**

The Blue Team protects systems and data, monitors for suspicious behaviour, and responds to incidents. It may include SOC analysts, incident responders, threat hunters, security engineers, and other defensive specialists.

## Task 2: Areas of Defensive Security

### Security Operations Centre (SOC)

A SOC monitors security telemetry from endpoints, servers, identity systems, networks, and applications. Analysts triage alerts, correlate events, identify likely incidents, and escalate activity that needs deeper investigation.

### Question: “What would you call a team of cyber security professionals that monitors a network and its systems for malicious events?”

**Answer: Security Operations Center (SOC).**

The room uses the American spelling, Center. The British spelling, Centre, refers to the same function.

### Digital Forensics and Incident Response (DFIR)

Digital forensics preserves and analyses evidence to understand what happened. Incident response contains the incident, removes the cause, restores operations, and records lessons learned. Evidence can include logs, files, memory, network traffic, and authentication records.

### Question: “What does DFIR stand for?”

**Answer: Digital Forensics and Incident Response.**

DFIR teams need to preserve evidence carefully and separate confirmed observations from hypotheses. A good timeline combines multiple sources rather than relying on a single log entry.

### Malware analysis

Malware analysis investigates suspicious files and behaviour to understand capabilities, indicators, and impact. Analysts may inspect hashes, strings, metadata, code, process activity, filesystem changes, and network connections. Samples should be analysed in an isolated environment, not run on a personal workstation.

### Question: “Which kind of malware requires the user to pay money to regain access to their files?”

**Answer: Ransomware.**

Ransomware commonly encrypts or otherwise blocks access to data and demands payment. Paying does not guarantee recovery. Defensive controls include offline or immutable backups, patching, least privilege, endpoint monitoring, and tested recovery procedures.

## Task 3: Follow Along

The room asks you to follow the introductory material and obtain the flag displayed by the exercise.

### Question: “What is the flag that you obtained by following along?”

Follow the active room's instructions and copy the flag shown by the task. The public repository intentionally redacts the value as `THM{hidden}`.

## How the concepts work together

A simple defensive workflow is:

1. **Detect:** monitoring tools raise an alert.
2. **Triage:** an analyst checks the alert's evidence and context.
3. **Investigate:** related logs and artefacts are correlated to establish what happened.
4. **Contain:** access or systems are restricted to stop ongoing harm.
5. **Eradicate and recover:** the root cause is removed and systems return to trusted operation.
6. **Improve:** detections, controls, and procedures are updated to reduce recurrence.

Not every alert is a confirmed incident. Analysts should verify evidence, record uncertainty, and escalate when the impact or complexity exceeds their authority.

## Key takeaways

The Blue Team provides defensive security. A SOC monitors systems for suspicious events, DFIR investigates and responds to incidents, and malware analysts help identify what malicious software does. Ransomware is a type of malware that can deny access to files and demand payment. The room's flag should be obtained from the live exercise and redacted in public notes.

## Expanded defensive workflow

### 1. Define the defensive objective

Defensive security aims to prevent, detect, investigate, and recover from attacks. The work may involve security operations, incident response, threat intelligence, endpoint security, identity protection, vulnerability management, and governance. The exact responsibilities vary by organization.

Begin by identifying the asset, the threat, and the control expected to reduce risk. A security product is not the goal by itself; the goal is to reduce the likelihood or impact of a real attack and prove that the control works.

### 2. Understand prevention, detection, and response

Prevention makes an attack harder or blocks it, for example by patching, least privilege, MFA, and network segmentation. Detection identifies suspicious activity through logs, endpoint telemetry, network sensors, and alerts. Response validates the alert, scopes the incident, contains the threat, removes persistence, and restores normal operation.

No single control is perfect. A layered design assumes that some controls will fail and provides additional opportunities to detect and contain an attack.

### 3. Security operations and logs

A SOC analyst gathers context from authentication, endpoint, firewall, DNS, proxy, and application logs. Useful questions include: which account acted, from which host, at what time, against what resource, and whether the behaviour is normal for that user or system.

A log entry is evidence of an event recorded by a particular source, not necessarily the complete truth. Check timestamps, host identity, event IDs, surrounding activity, and other sources. Preserve the original log and record the query or filter used.

### 4. Threat intelligence

Threat intelligence turns observations into context that helps defenders prioritize action. Indicators can include IP addresses, domains, URLs, file hashes, email senders, and behavioural patterns. Indicators have lifetimes and false positives; verify source, confidence, timestamp, and relevance before blocking or escalating.

Tactical intelligence helps detect immediate activity, operational intelligence describes campaigns and techniques, and strategic intelligence supports risk decisions. Use the level appropriate to the question rather than treating every indicator as equally useful.

### 5. Vulnerability and asset management

Defenders need an accurate inventory of devices, applications, owners, exposed services, and software versions. Prioritize vulnerabilities using exploitability, exposure, asset importance, available mitigations, and operational constraints. A severity score alone does not explain business risk.

After patching, verify the installed version or configuration and confirm the affected service is still healthy. Record exceptions and deadlines instead of assuming a change succeeded.

### 6. Work through the room

For each task, identify whether it asks about prevention, detection, response, intelligence, or a specific defensive role. Use the room's own definitions and examples for exact answers. If the question asks how an alert should be handled, explain triage, evidence gathering, escalation, and containment rather than jumping immediately to deletion or rebooting.

### 7. Measure effectiveness

Useful measures include time to detect, time to contain, coverage of critical assets, patch compliance, alert quality, and recovery-test results. Metrics should be interpreted carefully: a high alert count may indicate more attacks, noisy detection, or both.

### 8. Key lessons

Effective defence combines people, process, and technology. Maintain asset visibility, enforce strong identity controls, centralize useful logs, tune detections, rehearse incident response, and validate recovery. The goal is not to claim a system is impossible to compromise, but to make attacks harder to execute and easier to detect and contain.
