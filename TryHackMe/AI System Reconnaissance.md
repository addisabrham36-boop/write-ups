# AI System Reconnaissance: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/aisystemreconnaissance

## Room Overview & Information

**Category:** AI security / reconnaissance  
**Difficulty:** Beginner to intermediate  
**Focus:** Identifying AI-enabled services, mapping exposed interfaces, fingerprinting model endpoints, and documenting an AI system's attack surface.

AI reconnaissance follows the same core discipline as web and network reconnaissance, but the assets include model endpoints, inference APIs, retrieval systems, agent tools, model registries, and data pipelines. The goal is to establish what exists and how it is exposed before testing whether any component is vulnerable.

Use only the room's assigned targets. AI endpoints can incur costs, process sensitive data, or invoke tools, so uncontrolled probing is not appropriate outside an authorised environment.

## Phase 1: Define scope and identify the service

Start the lab and record the current target address. If the room provides a hostname, use that exact hostname and follow the instructions for resolving it. Do not scan unrelated network ranges.

For an authorised machine, start with a service scan:

```bash
nmap -sC -sV -oN ai-recon-nmap.txt MACHINE_IP
```

- `-sC` runs Nmap's default scripts.
- `-sV` attempts to identify service versions.
- `-oN` saves a readable report.
- `MACHINE_IP` is the current lab target.

Use the scan to identify web services and API ports. A port number or banner is only a clue; verify the application behaviour before labelling a service as an AI endpoint.

## Phase 2: Map the web/API surface

Open the application in a browser and inspect the pages, API requests, JavaScript assets, and response headers. Browser Developer Tools → **Network** can reveal the endpoint paths and request formats used by the frontend.

For a simple endpoint check:

```bash
curl -i http://MACHINE_IP/
```

The `-i` flag includes response headers. If the service uses a hostname or non-default port, use the exact URL from the room.

When mapping an AI application, look for:
- Public chat or inference endpoints.
- API documentation or schema routes.
- Model metadata or version information.
- File-upload and document-ingestion routes.
- Retrieval or search endpoints.
- Plugin or tool configuration.
- Administrative routes and health checks.
- Authentication requirements and rate limits.

Do not assume every path exists. Record only endpoints actually observed in the room.

## Phase 3: Recognise AI service patterns

AI services may expose ordinary HTTP endpoints that accept JSON. Common request fields can include a prompt, messages array, model name, input text, or a list of documents. The exact schema is application-specific.

If an endpoint returns a JSON error, inspect the status code and response body. A 401 often indicates authentication is required, 403 indicates a refusal or permission boundary, and 404 may indicate an incorrect route. None of these status codes alone proves that the underlying service is secure.

Avoid sending private data during reconnaissance. Use harmless test input and keep requests small.

## Phase 4: Fingerprint without overclaiming

Evidence that a service uses AI may include a documented model endpoint, a model identifier in the response, application documentation, or an interface that performs inference. A generic chatbot-like response is not proof of which model or version is deployed.

Record:
- Observed URL and HTTP method.
- Authentication requirement.
- Input and output format.
- Visible model or service identifier, if any.
- Rate limits and error behaviour.
- Whether the endpoint appears to retrieve external data or invoke tools.
- Timestamp and source of the observation.

Separate facts from hypotheses. For example, “the API returned model name X” is an observation; “the system is vulnerable to prompt injection” requires a separate test.

## Phase 5: Map trust boundaries

Identify which components process user-controlled content and which have access to sensitive resources. An AI system may include a frontend, inference API, model server, retrieval database, document store, plugin service, and external tools. The important question is where data crosses from an untrusted source into a component with greater authority.

Examples of useful questions:
- Can an unauthenticated user query the model?
- Can the model retrieve private documents?
- Does the model have access to tools that change state?
- Are model files or configuration exposed?
- Does the API enforce authorisation independently of model instructions?
- Are logs or debug endpoints publicly reachable?

Do not execute a high-impact action just to see whether a tool is available. Inspect the interface and use the room's safe test cases.

## Phase 6: Working through the room

For each task, follow the current lab's prompt and inspect the actual service:
1. Identify the requested endpoint or asset.
2. Use the room-provided target and tools.
3. Read the request and response fields carefully.
4. Distinguish service discovery from vulnerability confirmation.
5. Submit only the value requested by the task.
6. Preserve evidence and redact flags or sensitive data in public notes.

Dynamic values such as ports, model names, endpoints, and flags must come from the active instance. Do not substitute guessed output from a different version.

## Phase 7: Defensive recommendations

- Require authentication and authorisation on sensitive endpoints.
- Keep model servers, registries, and administrative interfaces off public networks unless needed.
- Apply rate limits and request-size limits.
- Separate model inference from privileged tool execution.
- Restrict retrieval sources and enforce user permissions at query time.
- Avoid returning stack traces, secrets, or internal metadata in errors.
- Maintain an inventory of models, versions, data sources, and dependencies.
- Monitor unusual query volume, tool use, and access to sensitive data.

## Key takeaways

AI system reconnaissance is asset discovery plus trust-boundary mapping. Identify exposed services, inspect their interfaces, record what the evidence shows, and avoid treating an AI-like response as proof of a particular architecture or vulnerability. Good reconnaissance produces a reliable map for a scoped assessment.