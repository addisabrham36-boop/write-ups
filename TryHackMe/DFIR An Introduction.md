# DFIR: An Introduction — TryHackMe Walkthrough

Official room: https://tryhackme.com/room/dfirintro

## Room Overview & Information

**Category:** Defensive security / digital forensics  
**Focus:** Evidence preservation, investigation timelines, containment, eradication, recovery, and reporting.

Digital Forensics and Incident Response (DFIR) combines evidence analysis with actions that contain and recover from security incidents. Forensics helps establish what happened; incident response limits harm and restores safe operations. The priorities overlap, but they are not identical.

## Phase 1: Preserve evidence

Before an incident, organisations should know which logs exist, where they are stored, who can access them, and how to preserve them. During an investigation, record the source, collection time, collector, and handling history for each item.

Evidence may include endpoint logs, authentication records, network captures, DNS logs, firewall events, disk images, memory captures, suspicious files, email headers, browser artefacts, and configuration changes.

Preserve original evidence where possible and analyse working copies. Hashes help verify whether a file changed after collection, but a hash alone does not prove that evidence was collected correctly. Provenance and handling records still matter.

\`\`\`bash
file suspicious.bin
sha256sum suspicious.bin
stat suspicious.bin
\`\`\`

- **file** identifies the likely file type.
- **sha256sum** calculates a SHA-256 digest.
- **stat** displays filesystem metadata.

These commands do not prove a file is malicious by themselves.

## Phase 2: Build a timeline

Normalise timestamps and document time zones. Correlate records from multiple sources to reconstruct the likely sequence of events. Keep raw observations separate from the analyst's interpretation.

A timeline might begin with a suspicious login, followed by a process launch, access to sensitive files, and outbound network traffic. Each link should be supported by evidence. If a log source is missing, record that gap rather than inventing the missing event.

## Phase 3: Analyse common artefacts

- **Authentication logs** show when and how an account was used.
- **Process records** can reveal what executed and under which identity.
- **DNS and network logs** can show attempted or completed communication.
- **Browser and email artefacts** can help explain user interaction.
- **File metadata and hashes** support identification and comparison.
- **Memory** may preserve running processes, connections, and other volatile state.

No single artefact usually tells the complete story. Cross-check findings and account for logging gaps, clock drift, and normal administrative activity.

## Phase 4: Contain the incident

Containment reduces ongoing harm. Depending on the situation, it may involve isolating an endpoint, disabling a compromised account, blocking a malicious indicator, or restricting access to a service. The correct action depends on business impact and the risk of allowing activity to continue.

Containment should be coordinated with the system owner. Abruptly shutting down a host may destroy volatile evidence or disrupt critical operations. When immediate risk is high, preventing further damage may take priority, but document the decision.

## Phase 5: Eradicate and recover

Eradication removes the cause of compromise, such as malicious persistence, exposed credentials, or a vulnerable configuration. Recovery returns systems to trusted operation, often from verified backups or clean images.

Deleting one suspicious file may not be enough if an attacker also created accounts, scheduled tasks, or stole credentials. After recovery, verify that the original entry point is fixed, credentials are rotated where needed, and monitoring is increased.

## Phase 6: Reporting

A clear DFIR report should include the scope, timeline, evidence sources, findings, impact, actions taken, remaining uncertainty, and recommendations. Distinguish confirmed facts from hypotheses. Include enough detail for another analyst to reproduce the reasoning without exposing personal data or secrets unnecessarily.

## Phase 7: Working through the room

For each task, identify whether it concerns evidence preservation, analysis, containment, eradication, recovery, or lessons learned. Follow the room's instructions for artefacts and tools. If a task asks for a timestamp, hash, or filename, read it from the provided evidence instead of substituting an example value.

## Key takeaways

DFIR requires both speed and discipline. Incident response limits harm, while forensic methods preserve the evidence needed to understand the incident and prevent recurrence. A good investigation records what is known, what remains uncertain, and why each action was taken.