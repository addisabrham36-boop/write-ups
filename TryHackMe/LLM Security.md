# LLM Security: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/llmsecurity

## Room Overview & Information

**Category:** AI and application security  
**Focus:** Prompt injection, data disclosure, output handling, tool access, and secure deployment.

An LLM generates output from input and context. It does not automatically know which content is trusted or whether a requested action is authorised. Security depends on the surrounding application: what data is supplied, what tools are available, and which controls execute outside the model.

## Phase 1: Map the architecture

A deployed LLM application may include a frontend, API gateway, inference service, system prompt, retrieval-augmented generation (RAG), plugins, and external tools. Map these components before testing. The chat box is only one entry point; APIs, document uploads, retrieval, and administration routes may expose other attack surfaces.

## Phase 2: Prompt injection

Prompt injection occurs when untrusted content attempts to influence the model's instructions. It may be direct, through a user prompt, or indirect, through a webpage, email, PDF, or retrieved document.

In the authorised lab:
1. Use the harmless test case supplied by the room.
2. Record the exact input and model response.
3. Inspect whether a tool call was requested.
4. Verify whether the application authorised and executed it.
5. Confirm any disclosure or state change in the underlying system.
6. Document evidence and limitations.

A model's statement that it has accessed a file is not proof. Check the system that owns the file or action.

## Phase 3: Sensitive information disclosure

Private data can leak through model output, retrieval, logs, system prompts, or poorly scoped API responses. Do not store secrets in prompts and assume the model will keep them private.

For RAG systems, enforce user permissions before documents are retrieved and placed in context. Filtering only the final response may be too late if the model has already received protected content.

## Phase 4: Tool and agent security

Tools can expand an agent's capabilities to search files, call APIs, send email, run code, or modify records. Each tool should have a separate authorisation check and a narrow schema.

Use read-only access when possible, validate arguments, require human approval for high-impact actions, apply timeouts and quotas, and log tool requests and results. A prompt that tells a model not to delete files is not equivalent to removing its ability to delete files.

## Phase 5: Output handling

Treat model output as untrusted data. If it is inserted into HTML, SQL, shell commands, or source code, apply the correct context-specific protection.

- Use parameterised queries for SQL.
- Escape HTML according to its output context.
- Avoid shell commands built from model-generated strings.
- Validate structured output against a schema.
- Treat model-generated URLs and paths as untrusted.
- Verify claims before using them in high-impact decisions.

Valid-looking JSON is not necessarily safe or correct. Parse it and validate the fields.

## Phase 6: Model supply chain

Track model names, versions, hashes, dependencies, and approval status. Download artifacts from trusted sources and verify integrity before loading them. Some serialized formats can execute code during deserialization, so inspect untrusted artifacts using safe methods in an isolated environment.

Keep API keys out of source code and prompts. Use a secret manager and rotate credentials if exposure is suspected.

## Phase 7: Reporting and remediation

For each test, record the endpoint, user role, exact input, response, tool activity, evidence of actual impact, and recommended fix. Do not claim an exploit succeeded based only on generated text. Use only the lab target and redact flags or secrets in public notes.

The main controls are least privilege, server-side authorisation, controlled retrieval, safe output handling, human approval for high-impact actions, provenance tracking, and monitoring. No single system prompt or filter solves all LLM security risks.

## Expanded LLM application assessment

### 1. Map the complete system

An LLM application commonly includes a user interface, orchestration code, model endpoint, prompt template, retrieval or search layer, tools, memory, logs, and third-party integrations. Threats often arise at the connections between these components rather than in the model alone. Draw the data flow and mark which inputs are attacker-controlled and which identities have access to sensitive resources.

### 2. Treat prompts and retrieved content as untrusted

Prompt injection can appear in a direct user message or in indirect content such as a web page, uploaded file, or retrieved document. The key question is whether untrusted content can cause the application to violate a security boundary. A model following a strange instruction is not enough; assess whether it disclosed protected data or performed an unauthorized action.

### 3. Review tool permissions

Inventory tools and the operations each can perform. Check whether a tool can read files, access external services, modify records, execute code, or send messages. Use narrow server-side permissions, structured arguments, allowlists, resource limits, and independent authorization checks. Do not rely on a prompt telling the model to use tools safely.

### 4. Protect data and retrieval

Apply access controls before private content is retrieved and passed to the model. Partition data by tenant and user, prevent secrets from entering prompts unnecessarily, and verify that logs do not retain credentials or sensitive records without a defined need. A model refusing to reveal a document is not a substitute for retrieval authorization.

### 5. Handle output safely

Model-generated output can be incorrect, maliciously influenced, or malformed. Validate schemas, apply context-appropriate output encoding, and never pass arbitrary model text directly into a shell, SQL engine, HTML renderer, or privileged tool. Keep consequential actions behind policy checks and human approval where appropriate.

### 6. Availability, supply chain, and monitoring

Set request quotas, token budgets, timeouts, and concurrency limits. Track model versions, dataset provenance, package sources, and plugin changes. Log relevant decisions and tool calls while minimizing sensitive content. Test upgrades before deployment and maintain a rollback path.

### 7. Practical test evidence

For each room scenario, record the normal behaviour, the test input, the resulting model response, any downstream tool action, and whether that action was authorized. A text response alone is not proof that a backend action occurred. Use only the provided lab and avoid probing unrelated provider systems.

### 8. Room tasks and remediation

Use the active room's examples for exact question values. Map each issue to its root cause, violated security property, and enforcement point. Prefer server-side authorization, least privilege, safe parsers, output validation, model provenance, and incident monitoring over prompt-only mitigations. Redact flags as `flag{hidden}`.
