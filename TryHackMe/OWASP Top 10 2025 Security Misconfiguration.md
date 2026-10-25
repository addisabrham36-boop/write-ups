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

## Phase 6: Question and evidence checklist

The room's practical focuses on a user-management API that leaks debug information when given an invalid user ID.

1. Request a valid record using the endpoint described above.
2. Record the expected response shape.
3. Send the invalid identifier used by the task.
4. Inspect the error body for the flag and internal information.
5. Submit the exact flag from the active instance; the public copy uses THM{hidden}.

Don't assume a 500 response is the vulnerability by itself. The issue is the sensitive details returned to the client. A safe client-facing error may be 400 or 404, but it should not reveal stack traces, secrets, local paths, or debug state.

## Final remediation checklist

Disable production debug mode, validate identifiers, use generic error responses, and write detailed diagnostics only to restricted server-side logs. Add regression tests for malformed IDs and unexpected exceptions. Review other endpoints for shared error handlers or debug settings so the same disclosure cannot be reached through a different route.
