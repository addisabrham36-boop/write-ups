# AI Threat Modelling: TryHackMe Walkthrough

Room: https://tryhackme.com/room/aithreatmodelling

This room applies structured threat modelling to systems that include AI. The central idea is simple: before choosing controls, map the assets, components, data flows, trust boundaries, and actions. An AI model is only one component in that system.

## 1. Understand the scenario and assets

The room scenario describes enterprise AI deployments, including a customer-facing assistant, a recommendation engine, and a fraud-detection system. Each system has different assets and consequences if it fails.

I start an inventory with:

- User prompts and uploaded documents.
- System instructions and application configuration.
- Training data, model weights, and model versions.
- Retrieved documents, embeddings, and vector stores.
- API keys, service identities, and tool permissions.
- Logs, monitoring records, and evaluation datasets.
- Business actions triggered by model output.

For each asset, record its owner, sensitivity, where it is stored, who can access it, and what depends on it. This prevents the assessment from focusing only on the visible chatbot while ignoring the database or tools behind it.

## 2. Draw the architecture and trust boundaries

Map the request path. A retrieval-augmented assistant might receive a user prompt, retrieve documents, add those documents to model context, generate an answer, and optionally call a tool. Each transition is a possible trust boundary.

Ask these questions at every boundary:

1. Who can supply the input?
2. Is the input trusted or untrusted?
3. Is access checked before data is retrieved?
4. Can the data influence instructions or trigger an action?
5. What identity and permissions does the next component use?
6. What is logged, and could the log itself expose secrets?

The key is to document actual flows rather than assume the model is isolated. If the system has a plugin, database, API, or shell tool, include it in the diagram.

## 3. Apply STRIDE with AI-specific examples

STRIDE provides six broad threat categories:

- **Spoofing:** an attacker impersonates a user, service, or data source.
- **Tampering:** an attacker alters prompts, retrieved documents, training data, model artifacts, or outputs.
- **Repudiation:** an action cannot be reliably attributed because audit records are missing or incomplete.
- **Information disclosure:** the system exposes confidential prompts, documents, records, or credentials.
- **Denial of service:** excessive or costly requests exhaust model, API, or infrastructure resources.
- **Elevation of privilege:** an AI tool or service performs an action beyond the caller's authority.

AI examples need context. Prompt injection may contribute to tampering or privilege escalation; a poorly protected vector store can cause information disclosure; and unrestricted agent loops can contribute to denial of service. Do not force every issue into one category if several apply.

## 4. Add AI-specific frameworks

MITRE ATLAS provides a catalogue of adversarial techniques targeting AI and machine-learning systems. OWASP's LLM guidance helps identify application risks such as prompt injection, insecure output handling, sensitive information disclosure, excessive agency, and weaknesses in the supply chain.

Use frameworks as a vocabulary, not as a substitute for analysis. For each threat, connect the technique to a specific component and explain the consequence in this deployment.

## 5. Score and prioritise risks

For each threat, record:

| Field | What to write |
|---|---|
| Asset | What needs protection |
| Threat scenario | What an attacker could do and how |
| Weakness | Which control is missing or unreliable |
| Impact | Confidentiality, integrity, availability, safety, or business impact |
| Likelihood | How feasible the path is in this environment |
| Existing controls | What already reduces the risk |
| Recommended action | A specific control and owner |
| Verification | How the team will test that the control works |

Do not assign a high severity merely because a threat sounds sophisticated. Consider exposure, required access, ease of exploitation, and the consequence of success. A public chatbot with unrestricted access to sensitive internal documents deserves different priority from an offline prototype with synthetic data.

## 6. Choose controls that enforce policy

Examples of practical controls include:

- Enforce document-level authorisation before retrieval.
- Give each tool a narrow, explicit permission set.
- Validate tool arguments and perform authorisation checks inside the tool service.
- Require confirmation for high-impact actions.
- Separate secrets from prompts and model context.
- Version and verify models, datasets, and dependencies.
- Limit request rate, token budgets, and recursive agent calls.
- Log security-relevant actions while minimising sensitive content in logs.
- Test prompt injection, data leakage, unsafe tool use, and denial-of-service scenarios.

Prompt wording can improve behaviour, but it cannot guarantee that the model will obey policy. The application must enforce security rules independently.

## 7. Work through the room tasks

Read the scenario in each task and identify the component being discussed. Map the risk to the correct threat category or framework, then explain why the risk applies to that component. In the assessment scenario, do not produce a generic list of every AI risk. Tie each finding to a data flow, permission, or trust boundary in the described architecture.

For the final assessment, check that every finding has a concrete mitigation and a way to verify it. “Make the AI safer” is not an actionable recommendation; “check the user's document permissions before retrieval and add a test for cross-user access” is.

## What I took from this room

Threat modelling turns general AI concerns into a prioritised engineering plan. STRIDE helps classify failure modes, ATLAS helps describe AI-specific adversary techniques, and OWASP helps locate application risks. The useful output is a system map, specific threat scenarios, prioritised findings, and controls that can be tested.