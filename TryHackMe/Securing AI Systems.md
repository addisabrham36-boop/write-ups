# Securing AI Systems: TryHackMe Walkthrough

Room: https://tryhackme.com/room/securingaisystems

This room focuses on securing AI architecture before deployment. The main lesson is that traditional application controls remain necessary, but they are not enough when a system adds a model, prompt construction, retrieval, vector storage, and tools that can perform actions.

## 1. Map the architecture

Start by identifying the user interface, API, model, data stores, retrieval layer, tool layer, identity provider, and external services. Draw the direction of data flow and mark trust boundaries. For a retrieval-based assistant, for example, the request may travel from the user to the API, from the API to a vector store, from retrieved passages into the model context, and from the model to a tool service.

At each boundary, record which identity is used and what permissions apply. Do not assume that the model itself is an enforcement point.

## 2. Identify high-value assets

Consider:

- Training data and model weights.
- System prompts and application configuration.
- Private documents and vector embeddings.
- API keys, tokens, and service identities.
- Tool arguments and tool results.
- User data, audit logs, and evaluation records.
- Model registry and software dependencies.

For each asset, define the confidentiality, integrity, and availability requirements. A recommendation engine may need protection against manipulated inputs, while a support assistant may have a high confidentiality risk if it can retrieve customer records.

## 3. Review trust boundaries and risks

The common architecture weaknesses to look for include:

- A user can retrieve documents belonging to another user.
- Untrusted webpage or document text can influence privileged tool calls.
- The model has access to broad internal APIs it does not need.
- Generated output is passed directly into SQL, shell, HTML, or another interpreter.
- Secrets are placed in prompts or logs.
- Model artifacts or dependencies can be downloaded from untrusted sources.
- Agent loops can run without sensible limits.
- Security events are not logged, or logs store excessive sensitive data.

When documenting a finding, explain the path from the weakness to the impact. “Prompt injection is possible” is incomplete unless the report explains what data or action the attacker can influence.

## 4. Apply least privilege

Use a dedicated identity for each service and grant only the permissions required for its task. Separate read-only tools from tools that change state. Enforce access checks in the API or tool service, not only in the model's instructions.

For example, if the assistant can search a knowledge base, it should only retrieve documents the current user is allowed to see. If it can create a ticket, the ticket API should validate the caller and the allowed fields before writing anything.

Require explicit approval for high-impact actions such as payments, account changes, deletion, or privileged administration. Add rate limits and execution budgets to reduce runaway loops and cost-based denial of service.

## 5. Protect data and model supply chains

Use trusted model sources and versioned artifacts. Record hashes, dependency versions, owners, and approval status. Review datasets for provenance and sensitive data, and prevent unauthorised modification of model files or training pipelines.

Secrets should live in a managed secret store and should be scoped and rotated. Avoid embedding credentials in prompts, notebooks, sample data, or source control. Restrict access to vector databases and object stores as carefully as ordinary databases.

## 6. Validate inputs and outputs

Treat user prompts, uploaded documents, webpages, and retrieved content as untrusted. Input filters can help, but they cannot reliably detect every malicious instruction. The more important safeguard is to prevent untrusted content from directly controlling privileged operations.

Validate outputs according to their destination. Use parameterised database operations, context-aware HTML encoding, strict tool schemas, and allowlisted actions. A generated URL, query, or command should not be executed without independent validation.

## 7. Monitoring and testing

Log security-relevant events such as authentication, retrieval access decisions, tool calls, denied actions, model/version changes, and unusual request volume. Minimise the sensitive content captured in logs and restrict access to them.

Test at least:
- Cross-user access to retrieved documents.
- Prompt injection through direct prompts and retrieved content.
- Tool calls with missing, invalid, or unauthorised arguments.
- Data leakage through answers and logs.
- Excessive loops, oversized requests, and resource exhaustion.
- Tampered or unapproved model and dependency artifacts.

Every finding should have a retest. If a mitigation claims to block cross-user document access, test with two identities and verify that one cannot retrieve the other's records.

## 8. Room task workflow

For each architecture or scenario task, identify the component, trust boundary, and failure mode. Select the risk category that matches the evidence, then choose a control that actually enforces the required policy. In the final review, connect each finding to an affected component and a recommended mitigation rather than listing unrelated security advice.

I do not claim the lab's final review passed unless the live room confirms it. Use the task feedback to verify the current exercise's expected answers.

## What I took from this room

Securing an AI system means securing its architecture and permissions, not merely adding safety instructions to a model. Least privilege, access checks before retrieval, validated tool actions, protected artifacts, monitoring, and adversarial testing form the practical baseline.