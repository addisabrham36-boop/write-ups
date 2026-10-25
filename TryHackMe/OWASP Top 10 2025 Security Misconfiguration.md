# OWASP Top 10 2025: Security Misconfiguration

Official room: https://tryhackme.com/room/owasptopten2025two

## Room Overview & Information

**Category:** Web application security  
**OWASP category:** A02 Security Misconfigurations  
**Practical:** User-management API with an error handler that exposes debug information.

Security misconfiguration happens when a system is deployed with unsafe settings, unnecessary exposure, weak defaults, or incomplete controls. This task demonstrates how a debug/error path can leak internal information that should be available only to developers through protected logs.

## Phase 1: Start the practical

Start the room's machine and use the current IP address assigned by TryHackMe. The user-management API runs on port 5002:

```text
http://MACHINE_IP:5002/
```

Replace MACHINE_IP with your active target address.

## Phase 2: Inspect the normal API response

The page describes the API. Request a normal user record:

```bash
curl http://MACHINE_IP:5002/api/user/1
```

The endpoint returns a user record. This establishes the normal response format before testing error handling.

## Phase 3: Trigger the verbose error

The application indicates that the user ID must be numeric. Send an invalid value:

```bash
curl http://MACHINE_IP:5002/api/user/bad
```

Inspect the response. The vulnerable handler exposes debug information, including a flag in the error output. This is a misconfiguration because detailed internals are returned to the client rather than being kept in protected server logs.

### Question: “What's the flag?”

1. Open the API on port 5002.
2. Request the normal record at `/api/user/1`.
3. Request the invalid identifier at `/api/user/bad`.
4. Inspect the verbose error response and locate the flag.
5. Submit the value shown by the active room.

The flag is redacted as `THM{hidden}`.

## Phase 4: Explain the root cause

The issue is not that the API accepts a URL parameter. The problem is that invalid input triggers an error handler that exposes internal state. The response may reveal stack traces, variable values, configuration, file paths, or secrets.

A production service should validate the input and return a generic client-facing error. Detailed diagnostic data belongs in access-controlled server logs.

## Phase 5: Remediation

- Disable debug mode in production.
- Validate user IDs and reject malformed input.
- Return a generic error response without internal details.
- Log diagnostic information on the server with restricted access.
- Ensure secrets and flags are not included in exception messages.
- Add regression tests for invalid identifiers and unexpected exceptions.
- Review other endpoints for the same error-handling pattern.

## Key takeaways

Security misconfiguration can expose sensitive information without a complex exploit. The room's practical is solved by comparing a normal API request with an invalid one and reading the leaked flag from the error response. The fix is to validate input and keep detailed diagnostics out of client-facing responses.

## Expanded configuration audit

### 1. Establish the intended secure baseline

Record the exposed application, server, framework, and deployment configuration. Identify which features are needed and which are unnecessary. Misconfiguration may include default accounts, directory listings, debug mode, verbose errors, permissive cross-origin rules, open administrative endpoints, weak security headers, or excessive permissions.

### 2. Observe externally visible evidence

Inspect the page and response headers using the room's browser or a simple request:

```bash
curl -i http://TARGET_HOST/
```

The `-i` option displays headers and response body. Use the exact scheme, hostname, and port provided by the lab. A server banner can reveal implementation details, but a banner alone is not proof of an exploitable weakness.

### 3. Inspect errors and defaults

Trigger only the invalid input permitted by the room and examine whether the response exposes stack traces, filesystem paths, framework details, SQL errors, or secrets. Detailed logs should remain in protected server-side storage, while client-facing errors should be concise and non-sensitive.

Check default credentials only when the room explicitly provides that task and target. Do not attempt default logins on real systems.

### 4. Validate impact

For a suspected directory listing, debug endpoint, or open file, establish what data is actually accessible and whether it is sensitive. Do not claim compromise from the presence of a feature alone. Preserve a sanitized response excerpt as evidence.

### 5. Remediation

Disable debugging in production, remove unused components, change defaults, enforce authentication and authorization, reduce service privileges, apply security headers where appropriate, keep dependencies updated, and centralize logs safely. Maintain a repeatable hardened baseline and review configuration changes.

### 6. Room tasks

Use the live response to identify the specific misconfiguration and answer requested fields exactly. Distinguish server-side evidence from what is merely visible in the browser. Redact flags as `flag{hidden}`.
