# AI Threat Modelling: TryHackMe Walkthrough

Room: https://tryhackme.com/room/aithreatmodelling

## Room Overview & Information

This note covers the practical workflow, evidence to collect, common interpretation errors, and defensive lessons for AI Threat Modelling. Use the live room as the authority for task wording and machine-specific values. The repository does not include fabricated terminal output, credentials, or flags.

## 1. Define the system before listing threats

Start the room and read its scenario. Identify the system's purpose, users, data, model, surrounding services, and deployment environment. A threat model is not a generic list of scary possibilities; it explains how a particular asset could be harmed through a particular trust boundary.

Draw the flow from input to output: user, application, model, retrieval/data source, tools or plugins, external provider, and logs. Mark which components are trusted, which are user-controlled, and where identity and permissions are enforced.

## 2. Inventory assets and security objectives

Common assets include:
- Private user content and conversation history.
- Training and retrieval data.
- Model weights, configuration, and system prompts.
- API keys, service credentials, and provider tokens.
- Tool execution, file access, and downstream business actions.
- Availability, integrity, and reputation of the service.

For each asset, state the required security property. Confidentiality means unauthorised parties cannot read it; integrity means it cannot be changed without permission; availability means authorised users can use the service. Authenticity and accountability may also matter when model output triggers actions.

## 3. Identify trust boundaries

Trust boundaries commonly exist between the public user and backend, between tenants, between retrieved documents and model instructions, between the model and a tool, between the application and a third-party provider, and between development and production.

For each boundary ask:
1. What data crosses it?
2. Who controls that data?
3. What identity and authorization checks occur?
4. What validation is performed?
5. What happens if the receiving component is compromised or behaves unexpectedly?

A prompt is not an access-control boundary. If the model can retrieve a document, the retrieval layer must enforce whether the current user is allowed to read it. If the model can call a tool, the tool's server must validate the action independently.

## 4. Build threat scenarios

A useful scenario states the attacker, precondition, action, weakness, impact, and control. Examples include a malicious document attempting prompt injection, a user trying to retrieve another tenant's data, a poisoned model artifact entering a release pipeline, or repeated oversized requests exhausting inference resources.

Use a structured method such as STRIDE when useful:
- Spoofing: impersonating a user, service, or model publisher.
- Tampering: changing data, model weights, prompts, or tool arguments.
- Repudiation: denying an action because audit records are absent.
- Information disclosure: exposing prompts, retrieval data, or credentials.
- Denial of service: exhausting model or infrastructure resources.
- Elevation of privilege: gaining access to tools or data beyond the user's role.

AI-specific risks such as prompt injection, model extraction, training-data poisoning, and unsafe agent tool use can be mapped to these outcomes. The labels help organise analysis; they do not replace a concrete attack path.

## 5. Rate and prioritise the risks

Estimate likelihood and impact using the room's required scale. Record the reason for each rating rather than assigning numbers by intuition alone. A threat that can disclose cross-tenant data may have high impact; its likelihood depends on exposure, permissions, and the demonstrated controls.

Use a table to make the decision explicit:

| Threat | Asset | Preconditions | Impact | Existing control | Remaining action |
|---|---|---|---|---|---|
| Prompt injection into a tool-enabled agent | Tool credentials and data | Untrusted content reaches the agent | Unauthorized action | Tool allowlist, if present | Enforce server-side authorization |
| Cross-tenant retrieval | Private documents | Retrieval lacks ownership checks | Data disclosure | Tenant filtering | Verify authorization on every query |
| Malicious model update | Model integrity | Publisher or pipeline compromised | Hidden behaviour | Registry controls | Signed artifacts and review |
| Resource exhaustion | Service availability | Expensive requests are unrestricted | Outage or cost spike | Rate limit, if present | Budgets, quotas, timeouts |

Replace hypothetical controls with what the actual room states. This table is a method example, not a claim about the lab's implementation.

## 6. Select mitigations at the correct layer

Do not attempt to solve every threat with prompt wording. Apply controls where they can be enforced:
- Identity and authorization belong in the application and tool service.
- Data separation belongs in retrieval and storage access controls.
- Output validation belongs before data is passed to another interpreter or tool.
- Model integrity belongs in artifact provenance, signing, and release management.
- Availability controls belong in quotas, timeouts, rate limits, and resource budgets.
- Auditability belongs in tamper-resistant logs that avoid recording unnecessary secrets.

Test mitigations with both expected and adversarial inputs. A control is not verified merely because it exists in a design document.

## 7. Complete the room tasks

For each task, extract the system component and risk being asked about. Map the evidence to an asset, trust boundary, threat, impact, and mitigation. If the question asks for a risk rating, use the room's scale. If it asks for a specific value or flag, obtain it from the active lab rather than inventing it.

Keep notes about assumptions. A threat model is a snapshot of a system and should be revisited when new tools, data sources, model versions, or permissions are added.

## What I took from this room

Good AI threat modelling starts with the system and its trust boundaries, then follows concrete attack paths to assets and impact. Controls must be enforced by the component that owns the relevant permission, not left to the model's good intentions.
## Phase 5: Complete the Threat Model

A complete result should connect each risk to a specific asset, trust boundary, abuse path, impact, existing control, and recommended change. Use the room's own rating scale; do not invent a numeric score.

| Risk | Asset | Required proof | Mitigation |
|---|---|---|---|
| Prompt injection | Model instructions, retrieved data, tool access | Untrusted content causes a policy or trust-boundary violation | Treat retrieved content as untrusted; validate actions server-side |
| Data poisoning | Training data and model behaviour | Suspicious examples produce a repeatable targeted change | Provenance, access control, label review, trusted evaluation |
| Model extraction | Model IP and API budget | Query patterns can reproduce behaviour beyond intended access | Authentication, quotas, output controls, monitoring |
| Privacy leakage | Training and retrieval data | Sensitive content is disclosed to an unauthorised requester | Data minimisation, retrieval authorization, privacy tests |
| Model drift | Decision quality and security detections | Evaluation shows performance degrades over time | Continuous evaluation, monitoring, retraining review |
| Unsafe tool use | Downstream systems and credentials | Model output invokes an action without an independent policy check | Least privilege, schemas, server-side authorization, human approval |

Prioritise risks based on evidence, likelihood, and impact, then define a retest for each mitigation. Threat models should be revisited when the model, retrieval corpus, plugins, tool permissions, or deployment architecture changes.
