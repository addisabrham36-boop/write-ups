# Defensive Security Intro

Official room: https://tryhackme.com/room/defensivesecurityintro

## Room Overview & Information

**Category:** Defensive security  
**Difficulty:** Easy  
**Topics:** Blue Team, Security Operations Centre (SOC), Digital Forensics and Incident Response (DFIR), malware analysis, and ransomware.

This is a conceptual and interactive room. It does not require port scanning, initial access, or privilege escalation. Its workflow is to understand the defensive role, distinguish specialist teams, and answer the provided knowledge checks using the room's definitions.

## Phase 1: Reconnaissance & Enumeration of Defensive Functions

Defensive security focuses on preventing, detecting, investigating, and responding to harmful activity. Monitoring can detect an anomaly, investigation determines what likely happened, and incident response limits damage and restores trusted operation. These activities depend on useful logs, endpoint telemetry, identity records, and network evidence.

## Phase 2: Identify the Relevant Defensive Team

### Task 1: Introduction to Defensive Security

**Question:** “Which team focuses on defensive security?”

**Answer:** **Blue Team**.

The Blue Team protects systems and data, monitors for suspicious behaviour, and responds to incidents. It can include SOC analysts, incident responders, threat hunters, security engineers, digital forensics specialists, and other defensive roles.

### Task 2: Areas of Defensive Security

#### Security Operations Centre

A SOC monitors security telemetry from endpoints, servers, identity systems, networks, and applications. Analysts triage alerts, correlate events, identify likely incidents, and escalate findings for deeper investigation.

**Question:** “What would you call a team of cyber security professionals that monitors a network and its systems for malicious events?”

**Answer:** **Security Operations Center (SOC)**.

The room uses the American spelling, Center. Centre describes the same function.

#### Digital Forensics and Incident Response

Digital forensics preserves and analyses evidence to understand what happened. Incident response contains an incident, removes the cause, restores operations, and records lessons learned. Evidence can include logs, files, memory, network traffic, and authentication records.

**Question:** “What does DFIR stand for?”

**Answer:** **Digital Forensics and Incident Response**.

A DFIR investigation builds a timeline from multiple sources and separates observed facts from hypotheses. Evidence should be preserved carefully because ordinary activity can alter timestamps, volatile memory, and local logs.

#### Malware analysis

Malware analysis investigates suspicious files and behaviour to understand capabilities, indicators, and impact. Analysts may inspect hashes, strings, metadata, code, process activity, filesystem changes, and network connections. Unknown samples should be investigated in an isolated environment rather than executed on a personal workstation.

**Question:** “Which kind of malware requires the user to pay money to regain access to their files?”

**Answer:** **Ransomware**.

Ransomware commonly encrypts or otherwise blocks access to data and demands payment. Paying does not guarantee recovery. Strong recovery controls include offline or immutable backups, patching, least privilege, endpoint monitoring, and tested restoration procedures.

### Task 3: Follow Along

**Question:** “What is the flag that you obtained by following along?”

Follow the active room's instructions and copy the flag displayed by the exercise. The live value is not independently available in this write-up, so it is intentionally redacted as **THM{hidden}** rather than guessed.

## Phase 3: Apply the Defensive Workflow

A practical defensive sequence is:

1. **Detect:** monitoring tools raise an alert.
2. **Triage:** an analyst checks evidence and context.
3. **Investigate:** related logs and artefacts establish what happened.
4. **Contain:** affected accounts, systems, or network paths are restricted to stop ongoing harm.
5. **Eradicate and recover:** the root cause is removed and systems return to trusted operation.
6. **Improve:** detections, controls, and procedures are updated to reduce recurrence.

Not every alert is a confirmed incident. Validate evidence, record uncertainty, and escalate when the impact or complexity exceeds your authority. Containment actions can disrupt business operations, so follow an approved playbook.

## Phase 4: Room Questions & Answer Summary

| Question | Answer |
|---|---|
| Which team focuses on defensive security? | **Blue Team** |
| What is the team that monitors systems for malicious events? | **Security Operations Center (SOC)** |
| What does DFIR stand for? | **Digital Forensics and Incident Response** |
| Which malware demands payment to restore access to files? | **Ransomware** |
| What is the flag after following along? | Read from the active room; redacted as **THM{hidden}** |
