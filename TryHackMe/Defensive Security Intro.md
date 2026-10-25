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