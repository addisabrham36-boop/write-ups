# Software Development Lifecycle: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/sdlc

## Room Overview & Information

**Category:** Software development and security  
**Difficulty:** Beginner  
**Objective:** Understand how software progresses from requirements and design through implementation, testing, deployment, maintenance, and retirement.

This is a conceptual room rather than a live exploitation target. Reconnaissance, initial access, and privilege escalation are not applicable to the standard SDLC lessons. Instead, the phases below map the same disciplined approach to the lifecycle of a software system.

## Phase 1: Requirements & System Enumeration

### 1. Identify users, assets, and requirements

Planning defines the business problem, user needs, constraints, and success criteria. Security requirements should be explicit and testable. Identify what data the system handles, who may access it, which actions are high impact, and what availability or privacy requirements apply.

A requirement such as “secure login” is too vague. Better requirements state how authentication is performed, how sessions expire, how access is authorised, what failed-login behaviour is expected, and which events are logged.

### 2. Inventory dependencies and trust boundaries

List external services, data stores, libraries, build systems, APIs, service accounts, and third-party components. Identify where data crosses a trust boundary and which component is responsible for validating it. This inventory becomes the baseline for threat modelling, testing, and dependency management.

## Phase 2: Design & Secure Implementation

### 3. Design before coding

Design defines architecture, data flow, component responsibilities, permissions, failure modes, and security controls. Threat modelling asks who may abuse the system, what asset is at risk, which preconditions are necessary, and which controls prevent the attack path.

### 4. Implement secure code

Use safe APIs, parameterized database queries, context-appropriate output encoding, proper authorization checks, and secure secret handling. Validate external input, but do not rely on validation alone when a safer API can preserve data/code separation. Keep secrets out of source files, public repositories, and build logs.

Use maintained dependencies from trusted sources. Pin and review changes where appropriate, and understand that a dependency update can change application behaviour as well as fix bugs.

## Phase 3: Testing & Verification

### 5. Choose appropriate test methods

- **Unit tests:** check small pieces of behaviour.
- **Integration tests:** check interactions between services and components.
- **Static analysis:** identifies classes of risky source patterns.
- **Dependency scanning:** checks packages against known vulnerability data.
- **Dynamic tests:** exercise a running application.
- **Manual security review:** examines business logic and trust assumptions that automated tools may miss.

Each test should have a defined expected result. A security finding should be reproduced safely and documented with the relevant input, output, impact, and limitation.

### 6. Add regression coverage

When a vulnerability is fixed, write a test that fails for the old behaviour and passes for the corrected behaviour. Run it during future changes so the same class of flaw is less likely to return. A passing test suite is evidence about tested cases, not proof that no vulnerabilities exist.

## Phase 4: Deployment, Operation & Incident Response

### 7. Release securely

Production configuration should minimise exposed services, protect secrets, use supported TLS settings, and grant only necessary permissions. Restrict who can change build pipelines and deployments. Preserve traceability from source commit to release artifact.

### 8. Monitor and maintain

After release, monitor logs and alerts, patch dependencies, review privileged access, rotate exposed secrets, and test backup restoration. During an incident, preserve evidence, scope the affected assets, contain the activity, recover from trusted sources, and document lessons learned.

### 9. Retire software safely

Decommissioning should revoke credentials, remove public endpoints, remove obsolete DNS and access grants, and handle retained data according to policy. Unmaintained software should be upgraded, isolated, or retired; leaving it online indefinitely increases risk.

## Phase 5: Room Questions & Key Takeaways

For each task, identify which lifecycle phase is being described. Requirements define what the system must do; design describes architecture and trust boundaries; implementation turns the design into code; testing verifies behaviour; deployment releases the system; maintenance keeps it secure; retirement removes it safely.

No question or flag should be guessed if it depends on a live quiz interface. Use the exact wording and answer options displayed by the current room. The key lesson is that security is continuous, beginning with requirements and ending only when the system and its credentials are safely retired.