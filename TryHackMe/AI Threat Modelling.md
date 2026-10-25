# AI Threat Modelling: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/aithreatmodelling

## Room Overview & Information

**Category:** AI security architecture  
**Focus:** Assets, data flows, trust boundaries, attacker goals, LLM threat categories, and mitigations.

Threat modelling identifies how a system can fail before an attacker discovers the weakness. For AI applications, the model is only one component. Data sources, retrieval, prompts, tools, identities, APIs, model artifacts, and deployment pipelines all affect risk.

## Phase 1: Define the system

Document the system's purpose, users, and components:
- Frontend and API gateway.
- Model inference service.
- System prompt and templates.
- Training or fine-tuning data.
- Retrieval sources and vector databases.
- Tools and plugins the model can invoke.
- Secrets and sensitive records.
- External dependencies and update channels.
- Logging and administrative interfaces.

Identify what the system must protect and what actions it is allowed to perform. A system that drafts text has a different risk profile from an agent that can modify production records.

## Phase 2: Identify assets and trust boundaries

High-value assets may include credentials, customer records, proprietary documents, model weights, API keys, training data, and tool permissions.

Trace data from user input to output. Mark where untrusted text is retrieved, where the model sees privileged context, where tool calls are authorised, and where data leaves the environment. For each boundary, ask who controls the input, what validation occurs, which identity is used, and what the next component can access.

## Phase 3: Map AI-specific risks

- **Prompt injection:** user input or retrieved content attempts to change model behaviour.
- **Sensitive information disclosure:** private data is revealed through outputs, retrieval, logs, or memorisation.
- **Supply-chain compromise:** a model, dataset, plugin, or dependency is tampered with.
- **Data/model poisoning:** manipulated data influences model behaviour.
- **Improper output handling:** model output is trusted as safe code, HTML, SQL, or commands.
- **Excessive agency:** the model can invoke tools beyond the task's needs.
- **System prompt leakage:** hidden instructions or secrets are exposed.
- **RAG access-control failure:** retrieval includes documents the current user should not see.
- **Resource exhaustion:** expensive or repeated requests degrade availability.

Map each risk to a component and a plausible attack path. Do not mark every category as applicable without evidence.

## Phase 4: Build a threat scenario

Example: indirect prompt injection through a retrieved document.

1. An attacker can influence a document later indexed by the application.
2. The retrieval system includes that document in model context.
3. The document contains instructions intended to redirect the model.
4. The model requests a tool call or attempts to disclose information.
5. The application fails to enforce permissions independently.
6. A sensitive action or disclosure occurs.

The root cause is the combination of untrusted retrieved content, model behaviour, tool authority, and missing application-level authorisation. The fix is not simply to tell the model to ignore malicious instructions.

## Phase 5: Prioritise risk

Assess likelihood, impact, exposure, and existing controls. A public endpoint with access to sensitive tools deserves more attention than an isolated model that only drafts text. Document assumptions and uncertainty so other reviewers can understand the rating.

## Phase 6: Test and verify

In the authorised lab:
1. Use the supplied harmless test input.
2. Record the exact prompt and model response.
3. Check whether a tool call was requested.
4. Verify whether the tool actually executed.
5. Confirm any state change in the system that owns the data.
6. Document the impact and remaining uncertainty.

A model saying it accessed a file does not prove that it did. A tool request is not proof that the tool executed successfully.

## Phase 7: Mitigations

- Apply least privilege to model accounts and tools.
- Enforce authorisation in application code.
- Treat user input and retrieved content as untrusted.
- Restrict retrieval to documents the current user may access.
- Require approval for destructive or high-impact actions.
- Verify model and dataset provenance.
- Validate output before passing it to interpreters.
- Monitor tool calls and unusual access patterns.
- Red-team the full workflow, not just the model response.

## Key takeaways

Threat modelling is a repeatable process: define the system, identify assets, trace data flows, mark trust boundaries, create attack scenarios, prioritise risk, and assign controls. AI risks become easier to reason about when they are mapped to the surrounding application rather than treated as mysterious model behaviour.