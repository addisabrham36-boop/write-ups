# AI System Reconnaissance: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/aisystemreconnaissance

## Room Overview & Information

**Category:** AI security reconnaissance  
**Focus:** Discovering AI-enabled services, mapping APIs, identifying model endpoints, and documenting trust boundaries.

AI reconnaissance follows familiar network and web-testing methodology, but the assets can include inference APIs, model registries, retrieval systems, document ingestion, plugins, and agent tools. Use only the target deployed by the room. Do not send private data or invoke high-impact actions during reconnaissance.

## Phase 1: Establish scope and scan

Start the lab and record its current IP. Use the room's hostname mapping if one is provided. An initial scan can identify reachable services:

```bash
nmap -sC -sV -oN ai-recon-nmap.txt MACHINE_IP
```

- `-sC` runs Nmap's default scripts.
- `-sV` attempts to identify service versions.
- `-oN` saves a readable report.
- `MACHINE_IP` is the active lab address.

Use scan results as leads, not proof of a vulnerability. A banner may be missing or inaccurate, and a port alone does not prove the service is an AI endpoint.

## Phase 2: Inspect the web/API surface

Open the web service and inspect the page, headers, and browser Network tab. Look for request paths, methods, JSON fields, authentication requirements, file uploads, API documentation, model metadata, retrieval routes, and administrative functions.

For a basic HTTP check:

```bash
curl -i http://MACHINE_IP/
```

The `-i` option includes response headers. Use the exact scheme, port, and hostname provided by the room.

Record each endpoint you actually observe. Do not invent paths based on a familiar framework.

## Phase 3: Identify AI components

An AI-enabled service may expose a chat endpoint, an inference API, a model identifier, a document-search interface, or a tool/plugin configuration. Identify what the evidence supports. A chatbot-like response does not reveal which model is running, and a model name in a response may not be a verified deployment version.

For each endpoint, record:
- URL and HTTP method.
- Required authentication.
- Input and output format.
- Model or service identifier, if exposed.
- Error behaviour and rate limits.
- Whether the endpoint retrieves documents or invokes tools.
- Timestamp and evidence source.

## Phase 4: Map trust boundaries

Trace how user-controlled data moves through the application. A typical flow may include a frontend, API, model server, retrieval database, document store, and tools. Mark where untrusted prompts or documents enter and which component has permission to access sensitive data.

Useful questions:
- Can an unauthenticated user query the service?
- Does retrieval enforce the current user's permissions?
- Can the model invoke tools that change state?
- Are model files or configuration exposed?
- Does the application enforce authorisation independently of the model?
- Are debug endpoints or stack traces public?

Do not test a high-impact action just to see if it is available. Use the room's safe examples.

## Phase 5: Work through the room

For each task, identify the requested endpoint or asset, use the active lab target, inspect the response, and submit the value requested. Distinguish service discovery from vulnerability confirmation. If a question asks for a live value, read it from the current instance rather than copying a result from another learner.

## Phase 6: Defensive recommendations

Require authentication and server-side authorisation for sensitive endpoints. Keep model registries and administrative interfaces private, apply rate limits, isolate model execution, restrict retrieval sources, and log tool calls. Monitor unusual request volume, repeated failures, and access to sensitive data.

## Key takeaways

AI system reconnaissance is asset discovery plus trust-boundary mapping. Identify the exposed services, inspect their interfaces, document evidence, and avoid assuming that a service is vulnerable simply because it uses AI.