# Securing AI Systems: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/securingaisystems

## Room Overview & Information

**Category:** AI security architecture  
**Focus:** Secure data handling, model provenance, inference endpoints, retrieval, agent tools, and operational monitoring.

An AI system includes more than a model. Its security depends on the data it receives, the way it is trained, the software around it, the APIs it exposes, the tools it can call, and the people who administer it. A secure design treats the model as one component inside a larger system of controls.

## Phase 1: Define the security objectives

Identify the assets and goals the system must protect:
- Confidentiality of user and business data.
- Integrity of training data, model artifacts, and outputs.
- Availability of inference services.
- Correct identity and permissions for every user.
- Accountability for model requests and tool actions.
- Provenance of models, datasets, and dependencies.

A system that drafts summaries has a different risk profile from an agent that can modify production records or execute commands. Define permitted actions before choosing controls.

## Phase 2: Secure the data lifecycle

Data security begins at collection and continues through training, evaluation, deployment, and deletion.

1. Collect only data that is necessary and authorised.
2. Review provenance and licensing.
3. Remove secrets and minimise personal data.
4. Restrict access to datasets.
5. Separate training, validation, and test data.
6. Version datasets and record transformations.
7. Scan for poisoning and anomalous records.
8. Define retention and deletion procedures.

Data poisoning can alter model behaviour, while leakage between training and test sets can make evaluation appear stronger than real-world performance.

## Phase 3: Protect model artifacts and dependencies

Maintain an inventory of models, libraries, datasets, plugins, and deployment images. Record version, source, hash, license, and approval status. Verify signatures or checksums using a trusted reference before deploying an artifact.

Do not load an unknown serialized model on an everyday workstation. Some formats rely on unsafe deserialization and may execute code. Use trusted formats and safe loading methods, and isolate analysis of untrusted artifacts.

Protect build pipelines and model registries with strong authentication, least privilege, code review, and audit logging. A compromised dependency can undermine a model even if the model's own logic is correct.

## Phase 4: Secure inference endpoints

For every inference API:
- Require authentication where appropriate.
- Enforce per-user and per-tenant authorisation.
- Apply rate and request-size limits.
- Validate the input schema.
- Restrict output size and execution time.
- Avoid exposing detailed stack traces.
- Monitor unusual request volume and access patterns.
- Keep administrative endpoints private.

A public endpoint is not automatically vulnerable, but it must be designed for its intended audience and abuse model.

## Phase 5: Secure retrieval-augmented generation

RAG applications retrieve documents and place them into model context. The retrieval layer must enforce the user's access permissions before content is supplied to the model. Otherwise, a user may ask questions that cause the model to reveal information from documents they could not access directly.

Treat retrieved documents as untrusted content because they may contain indirect prompt-injection instructions. Separate trusted instructions from retrieved data, limit the model's authority, and verify sensitive actions in application code.

## Phase 6: Restrict tools and agent authority

If the model can invoke tools, use least privilege:
- Define a small set of explicit tools.
- Validate arguments against strict schemas.
- Authenticate each tool call.
- Check authorisation independently of the model.
- Require human approval for destructive or high-impact actions.
- Apply network and filesystem restrictions.
- Log tool requests and results.
- Use timeouts, quotas, and emergency shutdown controls.

A prompt telling the model not to delete files is not equivalent to removing its ability to delete files.

## Phase 7: Monitoring and incident response

Log relevant requests, model and prompt versions, retrieval identifiers, tool calls, permission decisions, and errors. Avoid logging secrets unnecessarily. Centralise logs and restrict access so an attacker who compromises an application cannot easily erase the evidence.

During an incident:
1. Preserve relevant logs and artifacts.
2. Identify affected users, models, datasets, and tools.
3. Determine whether the model only generated unsafe text or whether an action was executed.
4. Contain the affected endpoint or tool integration.
5. Rotate exposed credentials and revoke sessions where needed.
6. Rebuild from trusted artifacts if integrity is uncertain.
7. Add regression tests and monitor for recurrence.

## Phase 8: Work through the room

Follow the task instructions and use the supplied examples. For each security control, identify the threat it addresses and how to verify the control. If a task asks for a concrete value or flag, retrieve it from the live environment and redact it in public notes.

Do not claim a control works merely because it is described in a policy. Test that unauthorised requests are rejected and that the system logs relevant failures.

## Key takeaways

Securing AI requires layered controls across data, models, APIs, retrieval, tools, and operations. The most important principles are least privilege, verified provenance, server-side authorisation, safe output handling, strong monitoring, and explicit human oversight for high-impact actions.