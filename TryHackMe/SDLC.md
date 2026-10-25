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

## Expanded secure SDLC workflow

### 1. Requirements and design

Define functional requirements alongside security, privacy, availability, and audit requirements. Identify assets, users, trust boundaries, misuse cases, and operational constraints before implementation. Security requirements should be testable, not limited to "the system must be secure."

### 2. Implementation

Follow secure coding standards, use parameterized database access, validate inputs, encode outputs for their context, and keep secrets out of source control. Review dependencies and choose maintained libraries. Code review should examine authorization and error handling, not only style.

### 3. Testing

Use unit and integration tests for business rules, static analysis for risky code patterns, dependency scanning for known vulnerable packages, and dynamic testing for runtime behaviour. Integrate security testing throughout development rather than postponing it until release day.

### 4. Build and deployment

Protect CI/CD credentials, restrict who can modify build pipelines, pin dependencies where appropriate, produce traceable artifacts, and prevent unreviewed changes from reaching production. Separate development, test, and production environments and credentials.

### 5. Operations and maintenance

Monitor logs and alerts, patch vulnerabilities, rotate exposed credentials, review access, and test backups and recovery. Retirement should include revoking secrets, deleting unneeded data safely, and removing obsolete DNS entries or services.

### 6. Threat modelling and risk

For each feature, ask who can abuse it, which asset is at risk, how the abuse path works, and which control prevents it. Prioritize by likelihood and impact; document exceptions and reassess when the design changes.

### 7. Room tasks

Map questions to the lifecycle phase or security activity they describe. Use the room's exact terminology and distinguish testing from deployment, monitoring, and maintenance.

### 8. Key takeaway

A secure SDLC makes security a continuous responsibility from requirements through retirement. Automation helps, but ownership, review, evidence, and the ability to fix findings are what make the process effective.
