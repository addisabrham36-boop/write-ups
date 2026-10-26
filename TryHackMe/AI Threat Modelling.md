# AI Threat Modelling

Official room: https://tryhackme.com/room/aithreatmodelling

## Room Overview & Information

**Category:** AI security and risk analysis  
**Objective:** Identify assets, trust boundaries, attacker goals, possible abuse paths, and controls in an AI-enabled system.

This is a threat-modelling exercise, not a machine exploitation room. A traditional recon-to-root chain is not applicable unless the active task introduces a specific target. Use the system described by the room and keep assumptions separate from observed facts.

## Phase 1: Reconnaissance & System Enumeration

### 1. Define the system

Inventory the user interface, application backend, model endpoint, retrieval service, data stores, tools/plugins, external providers, deployment pipeline, and monitoring systems. Draw the data flow from user input through model inference and any downstream action.

Record:
- Assets and sensitive data.
- User and service identities.
- External dependencies and data sources.
- Actions the model or connected tools can perform.
- Trust boundaries and points of authorization.
- Deployment and update mechanisms.

### 2. Identify assets and security objectives

Common assets include private conversation history, training and retrieval data, model weights, prompts, API keys, service credentials, tool execution, business records, and service availability.

Confidentiality prevents unauthorized reading; integrity protects against unauthorized change; availability supports legitimate use. Authenticity, accountability, and non-repudiation may also matter where AI output triggers actions.

## Phase 2: Threat Analysis and Attack Paths

### 3. Map trust boundaries

Common boundaries exist between the user and backend, between tenants, between retrieved documents and model instructions, between the model and a tool, between the application and external providers, and between development and production.

At every boundary, ask:
1. What data crosses it?
2. Who controls that data?
3. What identity and authorization checks are performed?
4. What validation or normalization occurs?
5. What happens if the receiving component behaves unexpectedly?

A prompt is not an access-control boundary. If the model can retrieve a document, the retrieval layer must check whether the current user can access it. If the model can call a tool, the tool service must independently validate authorization and arguments.

### 4. Create realistic threat scenarios

A scenario should name the attacker, preconditions, action, weakness, impact, and control. Examples include:
- A malicious document tries prompt injection in a tool-enabled agent.
- A user attempts to retrieve another tenant's private data.
- A poisoned model artifact enters the release pipeline.
- An unsafe generated value reaches a shell or SQL interpreter.
- Repeated expensive requests exhaust inference resources.

STRIDE can help organize threats:
- **Spoofing:** impersonating a user, service, or model publisher.
- **Tampering:** changing data, weights, prompts, or tool arguments.
- **Repudiation:** actions cannot be reconstructed because audit evidence is missing.
- **Information disclosure:** private prompts, data, or credentials are exposed.
- **Denial of service:** requests exhaust compute or service capacity.
- **Elevation of privilege:** access exceeds the user's role.

AI-specific issues such as prompt injection, model extraction, data poisoning, and unsafe tool execution should be mapped to the concrete asset and impact rather than listed without context.

## Phase 3: Risk Prioritisation & Control Validation

### 5. Rate risk

Use the scale provided by the room. Record why each likelihood and impact rating was chosen. Consider exposure, prerequisites, existing controls, affected asset importance, and ease of detection. A threat that could disclose cross-tenant information may have high impact, but its likelihood depends on the actual architecture and permissions.

Example risk register:

| Threat | Asset | Preconditions | Impact | Control to verify |
|---|---|---|---|---|
| Prompt injection | Data and tools | Untrusted content reaches agent | Unauthorized action | Server-side tool authorization |
| Cross-tenant retrieval | Private documents | Retrieval lacks ownership checks | Data disclosure | Per-user authorization on every query |
| Malicious model update | Model integrity | Publisher/pipeline compromised | Hidden behaviour | Signed artifacts and approval |
| Resource exhaustion | Availability | Expensive requests unrestricted | Outage or cost spike | Quotas, timeouts, and rate limits |

This is a template, not a claim that the room has those exact controls. Replace example values with the observed architecture.

### 6. Choose controls at the correct layer

Do not try to solve every problem through prompt wording:
- Identity and authorization belong in the application and tool service.
- Data separation belongs in retrieval and storage permissions.
- Output safety belongs before data reaches another interpreter or tool.
- Model integrity belongs in artifact provenance and release management.
- Availability controls belong in request quotas, timeouts, rate limits, and resource budgets.
- Auditability belongs in protected logs that avoid unnecessary secrets.

Test controls against both expected and adversarial input. A control is not verified merely because it appears in a design document.

## Phase 4: Room Questions & Threat Register

For each task, identify the asset, trust boundary, threat, impact, and mitigation requested. If the question asks for a rating, use the room's scale rather than inventing a new one. If it asks for a configuration value or flag, retrieve it from the live exercise.

Record assumptions, supporting evidence, residual risk, and the owner of the recommended mitigation. Do not claim the application is secure just because the design includes a control; verify how it behaves.
