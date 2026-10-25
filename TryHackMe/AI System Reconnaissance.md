# AI System Reconnaissance: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/aisystemreconnaissance

## Room Overview & Information

**Category:** AI security and application reconnaissance  
**Objective:** Map an AI application's public surface, API endpoints, model/retrieval dependencies, tool integrations, and security boundaries.

AI reconnaissance adds attention to models, retrieval and tool use to ordinary web application mapping. Only examine the hostname, IP addresses, APIs, and accounts explicitly in scope for the room.

## Phase 1: Reconnaissance & Enumeration

### 1. Confirm scope and identify the exposed surface

Start the room and record the assigned host, base URL, authorized account, and any scanning restrictions. Open the application normally. Note its visible pages, features, model interface, uploads, retrieval functions, plugins, and administrative screens.

Inspect common public resources when they exist:

```bash
curl -i https://TARGET_HOST/
curl -i https://TARGET_HOST/robots.txt
curl -i https://TARGET_HOST/sitemap.xml
```

- **-i** includes response headers and body.
- Replace TARGET_HOST with the exact hostname in the room.
- Use HTTP instead of HTTPS if that is what the lab provides.

A route name or server header is a clue, not proof that the endpoint is vulnerable. Do not publish cookies, authorization headers, or tokens in notes.

### 2. Inspect browser requests

Use the browser Developer Tools Network tab while using the application's normal features. Record API path, method, content type, request fields, and response shape. Redact credentials and personal data. If the room provides API documentation or source files, inspect those as part of the authorized environment.

## Phase 2: API and AI-Specific Vulnerability Analysis

### 3. Build an endpoint inventory

| Component | What to inspect |
|---|---|
| Chat endpoint | Input fields, session state, authentication |
| Retrieval endpoint | Source documents and per-user access controls |
| Upload endpoint | Allowed types, size limits, and processing path |
| Tool/plugin endpoint | Available actions, credentials, authorization |
| Admin/debug routes | Authentication, exposed configuration, errors |
| Model metadata | Public model/configuration information |

Record only the routes actually observed in the lab. Do not fabricate endpoints or blindly scan production services.

### 4. Inspect AI-specific exposure

Look for model identifiers, prompt templates, retrieval sources, conversation history, tool definitions, file processors, provider requests, and inference logs. The existence of these components does not prove a vulnerability.

Check where permissions are enforced. Retrieval should check access before private content is passed to the model. Each tool should independently verify identity, target resource, allowed operation, and argument schema. Client-side hiding of a feature is not authorization.

### 5. Active enumeration within authorized scope

If the room allows port scanning and provides a lab IP, a service scan may be useful:

```bash
nmap -sC -sV -oN ai-recon-nmap.txt TARGET_IP
```

- **-sC** runs Nmap's default scripts.
- **-sV** attempts service/version identification.
- **-oN** saves readable output.

Only scan the specified lab machine. A web-only task may not need Nmap. Do not test credentials unless the room explicitly authorizes it.

## Phase 3: Validate Findings & Post-Investigation Handling

For a suspected prompt-injection or tool issue, preserve the original input, altered input, model response, downstream tool call, and resulting application state. A changed answer is not by itself a security impact. Verify whether protected data was disclosed or an unauthorized action actually occurred.

A route returning HTTP 200 is not proof of a vulnerability. Record the status, relevant body, user identity, permission context, and any corroborating evidence. Stop after the minimum authorized validation.

No privilege escalation is implied by API mapping. If a specific lab provides an execution path, verify the effective identity before claiming a foothold or elevated access.

## Phase 4: Room Questions & Evidence Walkthrough

For each task, identify the requested page, header, route, model setting, retrieval source, or behaviour. Save the exact request and a sanitized response when permitted. Use the live room for exact endpoint names, values, and flags; do not make up answers based on common application patterns.

Separate confirmed facts from assumptions and record the time and tool version. Redact sessions and flags in public notes.

## Phase 5: Key Takeaways & Remediation

Reduce exposed surface area, remove debug routes, authenticate administrative APIs, enforce per-user retrieval authorization, restrict tool permissions, validate uploads, rate-limit expensive inference, and keep secrets out of client-side bundles and logs. Maintain an endpoint inventory and retest it when the model, plugin set, or orchestration code changes.

AI reconnaissance is ordinary application reconnaissance with extra attention to model, retrieval, and tool boundaries. The result should be an evidence-based map of what exists, who can access it, and which components need deeper testing.