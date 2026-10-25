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

## Phase 8: Validate the Finding and Complete the Room

For each exercise, preserve the normal input, test input, response, and the relevant application-side evidence. When the task involves prompt injection, record whether it only changed the generated text or caused a protected data disclosure or a downstream action. When the task involves retrieval, verify the user's actual access rights in the data layer. When tools are involved, record the tool name, validated arguments, authorization decision, and observed side effect.

| Test area | What would count as useful evidence | Correct control |
|---|---|---|
| Direct prompt injection | Model behaviour differs after malicious user instructions | Do not rely on prompt rules for security decisions |
| Indirect prompt injection | Retrieved content influences an action beyond its allowed purpose | Treat retrieved content as untrusted data |
| Retrieval disclosure | Data is returned to a user who lacks permission | Filter before retrieval using identity and tenant permissions |
| Unsafe tool invocation | A tool executes an unauthorized operation | Server-side authorization, schema checks, least privilege |
| Output injection | Model output is interpreted as executable or active content | Parameterized APIs and context-specific output encoding |
| Model artifact risk | Untrusted serialization or dependency can execute code | Trusted provenance, safe formats, isolation, integrity checks |
| Resource abuse | Requests exhaust time, memory, quota, or cost | Rate limits, budgets, timeouts, and concurrency limits |

A generated response claiming that a tool succeeded is not enough. Verify the downstream system state. If a task asks for a flag or environment-specific value, obtain it from the active room and do not substitute a guessed value from another instance.

## Final remediation checklist

1. Enforce identity and authorization outside the model.
2. Retrieve only information the current user may access.
3. Treat prompts, documents, and tool outputs as untrusted.
4. Restrict tool permissions and validate arguments independently.
5. Validate model output before sending it to another interpreter or system.
6. Protect model artifacts, dependency sources, API keys, and build pipelines.
7. Monitor inference, retrieval, and tool use while minimising sensitive log data.
8. Test the secure and insecure cases and preserve a regression test for every confirmed issue.
