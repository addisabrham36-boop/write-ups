# AI Security Threats: TryHackMe Walkthrough

Room: https://tryhackme.com/room/aisecuritythreats

## Room Overview & Information

This note covers the practical workflow, evidence to collect, common interpretation errors, and defensive lessons for AI Security Threats. Use the live room as the authority for task wording and machine-specific values. The repository does not include fabricated terminal output, credentials, or flags.

## 1. Establish the threat model

Start the lab and read its scenario. Identify the system, the assets it handles, the trust boundaries, the attacker-controlled inputs, and the security objective. A threat model is useful only when it is tied to a real system and a plausible abuse path.

For AI systems, assets can include private prompts, training data, model weights, credentials, connected tools, business records, and service availability. Trust boundaries may exist between the user and model, model and retrieval system, model and tool, and developer pipeline and deployed model.

## 2. Distinguish common threat classes

**Prompt injection** attempts to influence an AI system by placing instructions in user input or retrieved content. It is especially dangerous when the model can access private data or call tools. Treat retrieved documents and web pages as untrusted data, not privileged instructions.

**Sensitive information disclosure** occurs when a system reveals private data through its answers, logs, retrieval layer, or tool access. The root cause may be excessive permissions or poor data separation rather than a model defect.

**Insecure output handling** occurs when an application trusts model output and sends it directly to a shell, SQL query, browser, or HTML renderer without validation and context-appropriate encoding.

**Training-data or model poisoning** changes data or model behaviour before deployment. Investigate provenance, unexpected trigger behaviour, and changes to model artifacts.

**Model denial of service** targets expensive inference, long contexts, repeated requests, or resource-intensive tools. Rate limits, quotas, timeouts, and resource budgets reduce the impact.

**Supply-chain compromise** can affect packages, plugins, model weights, datasets, deployment images, and CI/CD credentials. A model's popularity is not proof of its integrity.

## 3. Analyse a prompt-injection scenario

If the task includes a prompt, retrieved document, or agent transcript, separate trusted system configuration from untrusted content. Identify the instruction the attacker wants the model to follow, the protected asset, and the missing control that makes the attack consequential.

Test only the provided lab input. Record the original behaviour, the altered input, and the output. A prompt that changes the tone of an answer is not by itself a security impact. The relevant question is whether the system crosses a boundary, such as disclosing protected content or performing an unauthorised tool action.

Defences include clear separation of instructions and data, least-privilege tool permissions, explicit confirmation for consequential actions, allowlisted tool arguments, and server-side authorization independent of the model's decision.

## 4. Analyse data disclosure and tool misuse

Trace how the model obtains information. If retrieval is involved, check whether access controls are applied before the retrieved content reaches the model. Hiding a document in the interface is not enough if the retrieval service can return it to any user.

For tool-enabled agents, inspect which tools are available, what credentials they use, and whether the server validates each action. Never grant an agent broader access merely because the prompt says it should act responsibly. The application must enforce authorization.

## 5. Evaluate outputs safely

Treat model output as untrusted input. Before passing it to another component, validate its schema, enforce limits, escape it for the destination context, and reject unexpected fields. For shell, database, and file operations, prefer fixed functions with structured arguments over arbitrary generated commands.

A robust test records the input, the model response, the downstream action, and whether the action was authorised. This reveals whether the weakness is in the model, orchestration code, permissions, or output handling.

## 6. Answer the room questions accurately

Map each question to the concept or evidence it asks for. Use the lab's scenario and results rather than assuming every AI system has the same vulnerability. When asked to classify an attack, identify the attacker-controlled input and the violated security property. When asked for a mitigation, state which control prevents the demonstrated path and where it is enforced.

Do not fabricate outputs or flags. Retrieve machine-specific answers from the active room and redact flags in public notes as `flag{hidden}`.

## 7. Mitigation priorities

1. Define assets and trust boundaries before deployment.
2. Apply authorization in application code, not in prompts alone.
3. Give tools the minimum permissions and narrowest input schema possible.
4. Treat retrieved content, uploaded files, and model outputs as untrusted.
5. Isolate execution environments and restrict network egress.
6. Apply request quotas, token limits, timeouts, and monitoring.
7. Version and verify models, datasets, plugins, and dependencies.
8. Red-team the whole application, including retrieval and tools, not just the model response.

## What I took from this room

AI security depends on the system around the model. Prompting matters, but access control, data handling, tool design, deployment integrity, and monitoring determine whether an unexpected answer becomes a real compromise.

## Phase 5: Key Takeaways & Remediation

The main goal is to connect every conclusion to observable evidence and enforce security controls at the correct trust boundary. Keep the original artefacts, record the exact commands and outputs from your own run, and redact any flag in public notes as `flag{hidden}`.
