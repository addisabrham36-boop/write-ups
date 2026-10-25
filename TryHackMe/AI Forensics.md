# AI Forensics: TryHackMe Walkthrough

Room: https://tryhackme.com/room/aiforensics

AI forensics applies digital-investigation methods to incidents involving AI tools and systems. The investigation may involve prompts, model outputs, retrieved documents, API requests, tool calls, model versions, and ordinary endpoint or cloud logs. The important part is to preserve the evidence and reconstruct what happened without treating the model's own explanation as proof.

## 1. Define the incident and preserve evidence

Before analysing an event, record the reported time window, affected application, account, system, and suspected behaviour. Preserve original logs and files where possible. Record where each item came from, when it was collected, who handled it, and whether any transformation was applied.

Useful evidence may include:

- User prompts and model responses.
- Conversation IDs and timestamps.
- Retrieval results and document identifiers.
- Tool names, arguments, permission checks, and tool results.
- API gateway and application logs.
- Authentication events and account activity.
- Model, prompt-template, and application versions.
- Network events, uploaded files, and endpoint telemetry.

Not every system stores every item. A missing prompt log does not prove that no prompt was sent, and a generated statement such as “I deleted the file” does not prove the file was deleted. Verify consequential actions in the system that performed them.

## 2. Build a timeline

Normalise timestamps carefully. Logs may use UTC, local time, or different clocks. Keep the original timestamp and document any conversion.

A useful timeline links the user request to what the application did:

1. An identity authenticated or an existing session was used.
2. A prompt or file was submitted.
3. The application retrieved data or called an external service.
4. The model produced a response or requested a tool action.
5. The application authorised or rejected that action.
6. The tool returned a result.
7. The system displayed a response or changed state.

Compare events across systems. For example, if the model output claims a database record changed, check the database audit log. If a tool call is absent, distinguish between the model suggesting an action and the application actually executing it.

## 3. Investigate prompt injection

Prompt injection may be direct, from the user, or indirect, inside a document, webpage, email, or retrieved record. To investigate it, preserve the exact input and identify which content was trusted versus untrusted.

Then ask:

- Did the model's behaviour change after it encountered the content?
- Did it reveal information from a protected source?
- Did it request or execute a tool call?
- Did the application check the user's authority before allowing the action?
- Did the tool independently validate the request?
- Is there evidence of a real state change, or only generated text?

A suspicious instruction in a document is evidence of an attempted injection, not automatically evidence that the attack succeeded. The actual impact depends on the model behaviour and the surrounding controls.

## 4. Handle evidence safely

AI logs can contain personal data, confidential documents, access tokens, and user secrets. Restrict access to the case material, redact sensitive content in reports, and follow retention policy. Do not upload confidential artefacts to a public AI service merely to get a summary.

If an artifact must be processed with an AI tool, document which service received it, what data was sent, and what output was produced. Preserve the original file and keep the AI-generated summary separate from analyst findings.

## 5. Practical investigation checklist

For a suspected AI-related incident, I would:

1. Preserve relevant logs and configuration versions.
2. Record the incident window and affected identities.
3. Build a timeline across authentication, application, model, and tool logs.
4. Identify all retrieved data and actions requested by the model.
5. Verify actual actions against authoritative system logs.
6. Determine whether access controls were bypassed or merely prompted to be bypassed.
7. Assess data exposure, integrity changes, service disruption, and scope.
8. Recommend containment, credential rotation, permission reduction, and additional logging where justified.
9. Document uncertainties and gaps in evidence.

## 6. Working through the room

Read the scenario and identify which artefact is being examined. If a question asks what happened, use the evidence and timeline. If it asks for a forensic principle, choose the answer that preserves original evidence, records provenance, or separates observation from interpretation.

For interactive tasks, follow the current room UI and compare the agent's or system's statements with the available records. The exact artefacts can change with the lab. I do not invent incident logs, outcomes, or flags that I have not observed.

## What I took from this room

AI incident response still depends on ordinary forensic discipline: preserve evidence, build a timeline, verify actions in the systems that performed them, and record uncertainty. Prompts and model outputs are important evidence, but the surrounding application, identity, retrieval, and tool logs often establish the real impact.