# The Hacker Methodology: TryHackMe Walkthrough

Room: https://tryhackme.com/room/hackermethodology

A methodology is a repeatable way to approach a security assessment. Tools matter, but a clear sequence helps me avoid missing obvious information, testing out-of-scope systems, or claiming a vulnerability without evidence.

## 1. Define the target and rules

Before testing, identify the target, permitted techniques, time limits, and any prohibited actions. In a training room, the deployed target and task instructions define the scope. A public IP or website is not automatically authorised for testing.

Record the target address and any required hostname mapping. Keep notes on the commands run and the observations returned so the work can be reproduced.

## 2. Reconnaissance

Gather initial information about the target. Passive reconnaissance uses public sources such as DNS, search results, and public documentation. Active reconnaissance interacts with the target, for example by making HTTP requests or scanning ports.

Separate verified facts from assumptions. A public certificate may reveal a hostname, but it does not prove the host is currently online. A service banner can help identify software, but it does not prove the software is vulnerable.

## 3. Enumeration

Enumeration takes the initial discoveries and investigates them in more detail. If a port is open, identify the service, version, authentication method, and relevant application paths. If a web application is present, inspect its pages, inputs, source, and responses.

Use a narrow tool for the question at hand. A full port scan finds reachable TCP ports; service detection helps identify what is listening; directory discovery finds possible web paths; and manual requests help validate behaviour. Save output and review it rather than running every tool without a plan.

## 4. Vulnerability analysis

Compare the observed service and configuration with reliable documentation and advisories. Check the conditions required for a vulnerability and whether they apply to the target. Version numbers can be misleading because vendors may backport fixes.

Prioritise weaknesses by exploitability, exposure, privileges required, and impact. A potential weakness should remain a hypothesis until the evidence supports it.

## 5. Controlled exploitation

Exploit only what the scope allows. Choose the least disruptive proof that establishes the weakness, and understand possible side effects before running a module or payload. After a successful test, verify the result through the actual target state, not merely a tool's success message.

If access is obtained, record the identity and privileges. Do not assume a low-privilege shell means administrator access, and do not collect unrelated private data.

## 6. Post-exploitation and cleanup

Post-exploitation checks the impact of access, such as whether a user can read sensitive files or cross a privilege boundary. Stay within the exercise's objectives. Remove test artefacts only as permitted and document changes made during testing.

## 7. Reporting

A strong finding contains the affected asset, vulnerability, evidence, impact, reproduction steps, severity rationale, and remediation. The report should explain the attack path in a way that another analyst can verify.

## What I took from this room

A consistent methodology makes security testing more reliable: define scope, discover the attack surface, enumerate services, validate weaknesses, demonstrate impact safely, and report actionable fixes.