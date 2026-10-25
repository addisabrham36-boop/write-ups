# The Hacker Methodology: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/hackermethodology

## Room Overview & Information

**Category:** Penetration-testing methodology  
**Difficulty:** Beginner  
**Focus:** Scope, reconnaissance, enumeration, vulnerability analysis, exploitation, impact assessment, and reporting.

A methodology is a repeatable way to approach a security assessment. Tools matter, but a clear sequence helps avoid missing obvious information, testing out-of-scope systems, or claiming a vulnerability without evidence.

## Phase 1: Define the target and rules

Before testing, identify the target, permitted techniques, time limits, and prohibited actions. In a training room, the deployed target and task instructions define the scope. A public IP or website is not automatically authorised for testing.

Record the target address, required hostname mapping, and any restrictions. Keep notes on commands and observations so the work can be reproduced.

## Phase 2: Reconnaissance

Gather initial information about the target. Passive reconnaissance uses public sources such as DNS, search results, registration data, and public documentation. Active reconnaissance interacts with the target, for example by making HTTP requests or scanning ports.

Separate verified facts from assumptions. A public certificate may reveal a hostname, but it does not prove the host is online. A service banner can help identify software, but it does not prove the software is vulnerable.

For an authorised lab, a basic scan can be recorded with:

\`\`\`bash
nmap -sC -sV -oN scan.txt TARGET_IP
\`\`\`

The default scripts gather common service information, service-version detection probes open ports, and the output file preserves the results. Use the live address from the room.

## Phase 3: Enumeration

Enumeration takes initial discoveries and investigates them in more detail. If a port is open, identify the service, version, authentication method, and relevant application paths. If a web application is present, inspect its pages, inputs, source, and responses.

Use a narrow tool for the question at hand:
- A full port scan finds reachable TCP ports.
- Service detection helps identify what is listening.
- Directory discovery finds possible web paths.
- Manual HTTP requests help validate application behaviour.
- SMB or DNS tools reveal service-specific information when those services are in scope.

Save output and review it rather than running every tool without a plan. Enumeration should reduce uncertainty and guide the next test.

## Phase 4: Vulnerability analysis

Compare the observed service and configuration with reliable documentation and advisories. Check the conditions required for a vulnerability and whether they apply to the target. Version numbers can be misleading because vendors may backport fixes without changing the public version string.

Prioritise weaknesses by exploitability, exposure, privileges required, and impact. A potential weakness remains a hypothesis until the evidence supports it.

For a CVE-based issue, verify the affected versions, required feature, configuration, and available patch. A proof-of-concept script may be written for a different target and may be unsafe to run without review.

## Phase 5: Controlled exploitation

Exploit only what the scope allows. Choose the least disruptive proof that establishes the weakness, and understand possible side effects before running a module or payload. After a successful test, verify the result through the target's actual state, not merely a tool's success message.

If access is obtained, record the identity and privileges. Do not assume a low-privilege shell means administrator access. Avoid accessing unrelated private data or making unnecessary changes.

## Phase 6: Post-exploitation and impact

Post-exploitation checks the impact of access, such as whether a user can read sensitive files or cross a privilege boundary. In a lab, follow the exact objective and stop when the required evidence is collected.

Useful verification questions include:
- Which account is running the process?
- What permissions does it have?
- Which system or container is the session on?
- Can the result be reproduced?
- What data or action is actually accessible?
- What remains unverified?

A command execution result does not automatically prove root access, and root inside a container does not automatically prove host compromise.

## Phase 7: Reporting

A strong finding includes the affected asset, vulnerability, evidence, impact, reproduction steps, severity rationale, and remediation. The report should explain the attack path in a way that another analyst can verify.

A useful finding separates:
- **Observation:** what the tool or application actually returned.
- **Interpretation:** why that behaviour indicates a weakness.
- **Impact:** what an attacker could do under the demonstrated conditions.
- **Remediation:** what change removes the root cause.
- **Retest:** how the fix can be verified.

## Phase 8: Cleanup and retesting

Stop listeners and temporary services, remove test artefacts where the scope permits, and record any changes made during the test. After remediation, repeat the same test to confirm the weakness is closed. A patch may not be sufficient if an insecure configuration remains.

## Key takeaways

A consistent methodology makes security testing more reliable: define scope, discover the attack surface, enumerate services, validate weaknesses, demonstrate impact safely, and report actionable fixes. The value is in the evidence and reasoning, not in the number of tools executed.