# DFIR: An Introduction: TryHackMe Walkthrough

Room: https://tryhackme.com/room/dfirintro

Digital Forensics and Incident Response (DFIR) combines evidence analysis with actions that contain and recover from security incidents. Forensics helps establish what happened; incident response limits harm and restores safe operations. The two activities overlap, but they have different priorities.

## 1. Prepare and preserve evidence

Before an incident, organisations should know which logs exist, where they are stored, who can access them, and how to preserve them. During an investigation, record the time, source, collector, and handling history for each item.

Useful evidence may include:
- Endpoint and operating-system logs.
- Authentication and cloud audit events.
- Network captures, DNS logs, and firewall records.
- Disk images and selected volatile memory.
- Suspicious files, email headers, and browser artefacts.
- Configuration changes, process history, and access records.

Preserve original evidence where possible and analyse working copies. Hashes can help verify that a file has not changed between collection and analysis. A hash alone does not prove the evidence was collected correctly, so provenance and handling records still matter.

## 2. Build a timeline

Normalise timestamps and document time zones. Correlate records from multiple sources to reconstruct the likely sequence of events. Keep the raw event and the analyst's interpretation separate.

A timeline may begin with an initial login, followed by a suspicious process, access to sensitive files, and outbound network traffic. Each link should be supported by evidence. If a log source is missing, record that gap instead of inventing the missing event.

## 3. Contain an incident

Containment reduces ongoing harm. Depending on the situation, it may involve isolating an endpoint, disabling a compromised account, blocking a malicious indicator, or restricting access to a service. The correct action depends on business impact and the risk of allowing activity to continue.

Containment should be coordinated with the system owner. Abruptly shutting down a host may destroy volatile evidence or disrupt critical operations. When immediate risk is high, preventing further damage may take priority, but the decision should be documented.

## 4. Eradicate and recover

Eradication removes the cause of compromise, such as malicious persistence, an exposed credential, or a vulnerable configuration. Recovery returns systems to trusted operation, often from verified backups or clean images. Merely deleting one suspicious file may not be enough if an attacker also created accounts, scheduled tasks, or stolen credentials.

After recovery, verify that the original entry point is fixed, credentials are rotated where needed, and monitoring is increased. A system that appears to work is not necessarily clean.

## 5. Analyse common artefacts

Different artefacts answer different questions:
- Authentication logs can show when and how an account was used.
- Process records can reveal what executed and under which identity.
- DNS and network logs can show attempted or completed communication.
- Browser and email artefacts can help explain user interaction.
- File metadata and hashes can support identification and comparison.
- Memory can contain running processes, network connections, and other volatile state.

No single artefact usually tells the complete story. Cross-check findings and account for logging gaps, clock drift, and normal administrative activity.

## 6. Reporting

A clear DFIR report should include the scope, timeline, evidence sources, findings, impact, actions taken, remaining uncertainty, and recommendations. Distinguish confirmed facts from hypotheses. Include enough detail for another analyst to reproduce the reasoning without unnecessarily exposing personal data or secrets.

## 7. Working through the room

For each task, identify whether it is asking about evidence preservation, analysis, containment, eradication, recovery, or lessons learned. Follow the room's lab instructions for artefacts and tools. When the task asks for a specific timestamp, hash, or filename, read it from the provided evidence instead of substituting an example value.

## What I took from this room

DFIR requires both speed and discipline. Response limits harm, while forensic methods preserve the evidence needed to understand the incident and prevent it from happening again.