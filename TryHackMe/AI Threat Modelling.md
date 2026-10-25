# AI Threat Modelling: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/aithreatmodelling

## Room Overview & Information

**Category:** AI security architecture  
**Difficulty:** Beginner to intermediate  
**Focus:** Assets, trust boundaries, attacker goals, LLM threat categories, attack paths, and mitigations.

Threat modelling is a structured way to identify how a system can fail before an attacker finds the weakness. For AI systems, the model itself is only one component. Data sources, retrieval, prompts, tools, identity, APIs, model artifacts, and the deployment pipeline all affect risk.

## Phase 1: Define the system

Start by describing the system's purpose and boundaries. Identify:
- Users and their roles.
- The frontend and API.
- The model or inference service.
- Training and fine-tuning data.
- Retrieval sources and vector databases.
- Tools or plugins the model can invoke.
- Secrets and sensitive records.
- External dependencies and update channels.
- Logging, monitoring, and administrative interfaces.

Do not begin with a list of attacks before you know what the system contains. A threat model without assets and data flows quickly becomes a list of generic security slogans.

## Phase 2: Identify assets and trust boundaries

High-value assets may include credentials, customer records, proprietary documents, model weights, API keys, training data, and the ability to perform actions through connected tools.

Draw the data flow from user input to output. Mark where untrusted text is retrieved, where the model sees privileged context, where tool calls are authorised, and where data leaves the environment. A key trust boundary exists whenever information from a less-trusted source enters a component with more privilege.

For each boundary, ask:
- Who controls the input?
- What validation occurs?
- Which identity is used?
- What can the next component access?
- Is the action authorised independently of the model?
- What evidence is logged?

## Phase 3: Map AI-specific risks

The OWASP Top 10 for LLM applications provides useful categories. Examples include:

- **Prompt injection:** user input or retrieved content attempts to change model behaviour.
- **Sensitive information disclosure:** private data is revealed through output, retrieval, logs, or memorisation.
- **Supply-chain compromise:** a model, dataset, plugin, or dependency is tampered with.
- **Data/model poisoning:** manipulated data influences training or model behaviour.
- **Improper output handling:** model output is trusted as safe code, HTML, SQL, or commands.
- **Excessive agency:** the model can invoke tools or actions beyond what its task requires.
- **System prompt leakage:** hidden instructions or secrets are exposed.
- **Vector/RAG weaknesses:** retrieval returns unauthorised or attacker-controlled content.
- **Misinformation:** unreliable output is presented as authoritative.
- **Resource exhaustion:** expensive queries or repeated requests degrade availability.

Map each risk to a specific component and attack path rather than marking every category as applicable to every system.

## Phase 4: Construct attack scenarios

A useful scenario includes a threat actor, entry point, weakness, impact, and evidence. For example:

**Scenario: Indirect prompt injection through retrieved documents**
1. An attacker can influence a document that the application later retrieves.
2. The retrieval system includes the document in the model's context.
3. The document contains instructions intended to redirect the model.
4. The model requests a tool call or exposes information.
5. The application fails to enforce permissions independently.
6. A sensitive action or disclosure occurs.

The root cause is not just “the model followed instructions.” It is the combination of untrusted retrieved content, model behaviour, tool authority, and missing application-level authorisation.

Another scenario is a compromised model artifact entering through an unverified registry. The key controls are provenance, signature verification, dependency review, and monitoring.

## Phase 5: Rate and prioritise risk

Prioritise threats by likelihood, impact, exposure, and existing controls. A public endpoint with access to financial or administrative tools deserves more attention than an isolated model that only generates draft text.

Record the assumptions behind each rating. A threat model is a working document, not a claim that every possible attack has been proven.

## Phase 6: Test the model and application separately

Model evaluation can test whether prompts influence the output. Application testing must verify whether the system permits an unauthorised action. These are different questions.

For a controlled lab:
1. Use a harmless prompt-injection test supplied by the room.
2. Observe the model's response.
3. Inspect whether a tool call was requested.
4. Verify whether the application authorised or rejected the action.
5. Confirm the actual result in the system that owns the data.
6. Record the evidence and any uncertainty.

A model saying it has accessed a file does not prove the file was accessed. A tool call is not proof the tool executed successfully. Verify the action in the underlying system.

## Phase 7: Mitigations

- Apply least privilege to model accounts and tools.
- Enforce authorisation in application code.
- Treat user input and retrieved content as untrusted.
- Separate data from instructions where possible and test for indirect injection.
- Require approval for destructive or high-impact actions.
- Use trusted model and dataset sources and record provenance.
- Validate model output before passing it to interpreters.
- Monitor tool calls, retrieval access, and anomalous query patterns.
- Red-team the full workflow, not only the model response.

## Working through the room

Use the task's system description and diagrams to identify assets, data flows, and trust boundaries. For each question, match the scenario to the specific threat and control. Do not invent an attack path that the task does not support.

## Key takeaways

Threat modelling is a repeatable process: define the system, identify assets, trace data flows, find trust boundaries, create attack scenarios, prioritise risk, and assign controls. AI-specific risks become clearer when they are mapped to the surrounding application rather than treated as mysterious model behaviour.