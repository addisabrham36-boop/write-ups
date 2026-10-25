# Software Development Lifecycle: TryHackMe Walkthrough

Room: https://tryhackme.com/room/sdlc

The Software Development Life Cycle (SDLC) describes the stages used to plan, build, test, deploy, and maintain software. Security is more effective when it is included throughout the lifecycle instead of being added only after a vulnerability is discovered.

## 1. Common lifecycle stages

Different organisations use different names, but the stages commonly include:

1. **Planning and requirements:** define users, business goals, constraints, and security requirements.
2. **Design:** choose the architecture, data flows, trust boundaries, and technologies.
3. **Implementation:** write code and review dependencies.
4. **Testing:** verify functionality, security, performance, and expected failure handling.
5. **Deployment:** release the application with secure configuration and controlled access.
6. **Maintenance:** patch dependencies, monitor behaviour, respond to incidents, and retire unsupported components.

Some teams use sequential phases; others use iterative or continuous delivery. The underlying security activities still apply.

## 2. Security in requirements and design

Requirements should specify which data is sensitive, who may access it, how users authenticate, what the system must log, and how failures should be handled. During design, identify trust boundaries, external services, and high-impact actions.

Threat modelling is especially useful before implementation because it can reveal missing authorisation checks or unsafe data flows while changes are still relatively cheap.

## 3. Secure implementation

Developers should validate input, use safe APIs, avoid hardcoded secrets, and follow language-specific secure coding guidance. Dependencies should come from trusted sources and be kept updated. Code review can catch logic errors that automated tools miss.

Input validation is not a universal replacement for contextual encoding or parameterised APIs. For example, SQL queries should use parameters rather than concatenating user input, and HTML output should be encoded for its context.

## 4. Testing and verification

Testing can include unit tests, integration tests, static analysis, dependency scanning, dynamic application testing, and manual security review. Each technique finds different classes of issues.

A test should have a clear expected result. When a vulnerability is fixed, add a regression test so the same failure is less likely to return. Passing a test suite does not prove the application is free of vulnerabilities, but it provides evidence about the cases that were checked.

## 5. Secure deployment

Production configuration should minimise exposed services, protect secrets, use appropriate TLS settings, and grant only the permissions the application needs. Build and deployment pipelines should restrict who can modify releases and should preserve traceability from source commit to deployed artifact.

Secrets should not be stored in public repositories or copied into build logs. Use managed secret storage and rotate credentials if exposure is suspected.

## 6. Maintenance and end of life

Software remains exposed after release. Teams need a process for monitoring advisories, patching dependencies, reviewing access, rotating secrets, and responding to new threats. Unsupported software should be upgraded, isolated, or retired.

When a system is decommissioned, revoke credentials, remove public endpoints, and handle stored data according to retention requirements.

## 7. Working through the room

For each question, identify which lifecycle phase is being described and what security activity belongs there. Requirements define the needed protections; design maps architecture and trust boundaries; implementation applies secure coding; testing verifies behaviour; deployment configures the live service; and maintenance keeps it secure over time.

## What I took from this room

Security is not a final checklist at the end of development. It is a set of activities that begin with requirements and continue through deployment, monitoring, patching, and retirement.