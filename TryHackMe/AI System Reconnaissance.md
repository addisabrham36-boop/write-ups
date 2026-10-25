# AI System Reconnaissance: TryHackMe Walkthrough

Room: https://tryhackme.com/room/aisystemreconnaissance

## Room Overview & Information

This note covers the practical workflow, evidence to collect, common interpretation errors, and defensive lessons for AI System Reconnaissance. Use the live room as the authority for task wording and machine-specific values. The repository does not include fabricated terminal output, credentials, or flags.

## 1. Scope and reconnaissance plan

Room: https://tryhackme.com/room/aisystemreconnaissance

Start the room and identify the assets and scope stated in its briefing. Reconnaissance should establish what the AI application exposes before any active testing. Record the base URL, permitted endpoints, test account, and restrictions. Do not probe external systems that are merely mentioned by the target.

The goal is to map the AI system's public surface: interface, API routes, model or provider information, documentation, authentication, file upload and retrieval features, plugins, and tool integrations. Separate confirmed facts from assumptions.

## 2. Passive inspection

Open the provided application normally. Inspect the visible pages, page source, JavaScript bundles, robots.txt, sitemap, and any API documentation linked by the application. These can reveal route names or technology hints, but a route name does not prove that it is accessible or vulnerable.

Useful checks against the in-scope host include:

```bash
curl -i https://TARGET_HOST/
curl -i https://TARGET_HOST/robots.txt
curl -i https://TARGET_HOST/sitemap.xml
```

Replace TARGET_HOST with the hostname explicitly provided by the room and use HTTP if that is what the lab uses. The `-i` option displays response headers with the body. Record status codes, redirects, cookies, and server headers without publishing session tokens.

If browser developer tools are available, inspect the Network tab while using the application's normal features. Note which API endpoint is called, the HTTP method, and the shape of the request and response. Redact authorization headers and tokens from notes.

## 3. Map the application and API

Create a simple endpoint inventory. For each endpoint, record its purpose, method, authentication requirement, user-controlled fields, and whether it invokes a model, retrieval system, or external tool.

| Item | Questions to answer |
|---|---|
| Chat endpoint | What input is sent? Is conversation state stored? |
| Retrieval endpoint | Which documents can the current user access? |
| Upload endpoint | Which types and sizes are accepted? |
| Tool or plugin | What action can it perform and under whose identity? |
| Admin or debug route | Is it documented, authenticated, and in scope? |
| Model metadata | Is the information intentionally public? |

Use the routes actually observed in the lab. Do not fabricate endpoints or blindly scan a production service. If the room provides an API collection or source file, inspect it as the authoritative lab artifact.

## 4. Identify AI-specific exposure

An AI application can expose more than a traditional web application. Look for the model identifier, system prompt handling, retrieval sources, conversation history, tool definitions, file-processing components, and external provider requests. The existence of one of these components is not automatically a vulnerability.

Test whether access controls are enforced at the server. For example, if a chat interface displays only a user's documents, determine whether the retrieval API also enforces ownership. If a model can call tools, determine whether each tool independently checks authorization and validates its arguments.

Prompt injection becomes a security issue when untrusted content can influence an action that crosses a trust boundary. Record the input and resulting behaviour, and distinguish a harmless answer change from unauthorized data access or an unsafe tool call.

## 5. Active enumeration within scope

If the room authorises active enumeration, use a small, controlled request rate and stay on the supplied host. For HTTP services, a service scan can help identify exposed ports:

```bash
nmap -sC -sV -oN ai-recon-nmap.txt TARGET_IP
```

Here, `-sC` runs Nmap's default scripts, `-sV` attempts service-version detection, and `-oN` saves human-readable output. Only run this against the lab IP and only if port scanning is permitted by the room. A web-only challenge may not require Nmap at all.

If you encounter authentication, do not try random credentials unless the task explicitly authorises credential testing. Instead, document the access boundary and use the supplied lab account.

## 6. Evidence and answer workflow

For each task question, identify the relevant page, header, route, configuration, or observed behaviour. Save the exact request and a sanitized response when permitted. Avoid claiming a route is vulnerable because it returns 200 OK; the status code only says the server returned a successful HTTP response.

Use the active lab for exact model names, endpoint values, tokens, and flags. Those values can change and should not be invented. Redact flags in the public write-up as `flag{hidden}`.

## 7. Hardening recommendations

Reduce the exposed surface, remove debug routes from production, authenticate administrative APIs, enforce per-user retrieval authorization, restrict tool permissions, validate uploaded content, rate-limit expensive model calls, and avoid leaking secrets in client-side bundles or logs. Maintain an endpoint inventory and retest it when the model, plugin set, or orchestration code changes.

## What I took from this room

AI reconnaissance is ordinary application reconnaissance with extra attention to model, retrieval, and tool boundaries. The best result is an evidence-based map of what exists, who can access it, and which components deserve deeper testing.
## Phase 5: Final Checklist and Remediation

Before closing the room, verify that the inventory separates confirmed exposure from assumptions. For each discovered route, record its purpose, HTTP method, authentication requirement, the identity used during the test, and whether it calls a model, retrieval service, or tool.

| Check | Evidence to preserve | Security question |
|---|---|---|
| Public interface | URL, redirects, response headers | Does it disclose unnecessary platform details? |
| API route | Method, path, request shape, status | Is the route intended and properly authenticated? |
| Model service | Model identifier and permitted input | Is access controlled and usage rate-limited? |
| Retrieval or upload | Data source and ownership checks | Can one user access another user's data? |
| Tool integration | Tool definition and authorization result | Does the tool enforce permissions independently? |
| Debug or metrics endpoint | Exposure and authentication state | Does it reveal secrets, prompts, or internal state? |

Do not invent a task answer if it depends on the lab's interactive state. When the room asks for a completion confirmation, use the site's visible feedback. Remediation should minimise public endpoints, enforce authorization at the service that owns the data, restrict tool capabilities, validate uploaded content, and monitor model/API usage.
