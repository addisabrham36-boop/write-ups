# PHP2Shell: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/cvephp2shell

## Room Overview & Information

**Category:** Web server vulnerability research / memory corruption  
**Difficulty:** Advanced technical challenge  
**Vulnerabilities:** CVE-2026-109618 (memory disclosure) and CVE-2026-109619 (remote code execution)  
**Focus:** PHP runtime internals, upload-progress handling, memory safety, Apache worker processes, ASLR, patching, and detection.

PHP2Shell is a runtime-level vulnerability chain. It is not an ordinary application bug: a trivial PHP page can be affected because the flaw exists in PHP's request-processing and memory-management behaviour. The room's goal is to understand the vulnerability and verify its impact inside the isolated lab.

Because this is a newly disclosed critical remote-code-execution issue, this public write-up focuses on the room's reconnaissance, root cause, evidence, and remediation. It does not reproduce a weaponised exploit script or a copy-paste remote execution payload.

## Phase 1: Confirm the lab environment

Start the machine provided by TryHackMe and follow the room's instructions for accessing the target. The current room uses a local lab service; do not substitute a public website or a different PHP server.

If the exercise provides a Docker-based environment, inspect the running container and its version using the commands supplied by the room. Typical local inspection commands are:

\`\`\`bash
docker ps --format '{{.Names}}  {{.Image}}  {{.Status}}'
docker exec CONTAINER_NAME php -v
curl -i http://127.0.0.1:8088/index.php
\`\`\`

- **docker ps** lists running containers.
- **--format** controls the displayed container name, image, and status.
- **docker exec** runs a command inside a container you administer.
- **php -v** reports the PHP runtime version.
- **curl -i** requests the local lab page and includes response headers.

Replace CONTAINER_NAME and the port with the values shown by the active room. Do not run Docker commands against a production host unless you are its authorised administrator.

If the application source is provided, inspect it only to establish what the application itself does. The room's example is deliberately simple; the key issue is in the PHP runtime rather than a complex application function.

## Phase 2: Understand the vulnerability chain

The room covers two related vulnerabilities:

- **CVE-2026-109618:** memory disclosure that can expose unintended worker-memory contents through an HTTP response.
- **CVE-2026-109619:** memory corruption that can potentially lead to remote code execution.

The chain uses PHP's upload-progress feature, which can record progress information for multipart file uploads in server-side session data. The feature is legitimate, but the vulnerable runtime can enter an inconsistent memory state when a memory-limit failure interrupts normal cleanup.

### Why the memory-limit bailout matters

PHP has a per-request memory limit. When a request exceeds it, PHP can perform an emergency bailout instead of completing normal cleanup. The vulnerability involves a timing window in which a pointer may remain in an invalid state after memory has been released.

A dangling or uninitialised pointer refers to memory that is no longer valid for the expected object. If later code uses that pointer, the result may include memory disclosure or memory corruption. The weakness is associated with CWE-824, Access of Uninitialised Pointer.

### Why worker reuse matters

In the affected Apache prefork plus mod_php deployment, Apache worker processes handle multiple requests over their lifetime. If a request leaves internal state inconsistent, a later request handled by the same worker may encounter that state again. The worker reuse behaviour is part of the vulnerability's exploitability in the room's configuration.

This is why the issue must be understood as a runtime and worker-lifecycle problem, not just a bad line in a PHP application.

## Phase 3: ASLR and memory disclosure

Address Space Layout Randomization (ASLR) changes where code and memory regions are mapped, making fixed addresses unreliable. The room demonstrates that the memory-disclosure stage can reveal address information from the vulnerable process. That information can weaken the protection ASLR is intended to provide.

A memory leak is not the same as remote code execution. The two CVEs have different roles in the chain: one provides information, while the other can corrupt execution state. A full impact assessment should distinguish the evidence for each stage.

## Phase 4: Validate the result in the lab

Follow the room's current instructions and use its provided proof mechanism to validate the impact. Keep the test inside the deployed environment. Verify the result from the lab's own output rather than relying only on an exploit tool's success message.

If the task provides a command-execution proof, record:
- The request or action that triggered the lab result.
- The response evidence.
- The execution identity reported by the environment.
- The affected PHP version and server configuration.
- Any errors or worker restarts observed.

A process running as the web-server account is still a serious compromise, but it is not automatically root. State the privileges actually demonstrated.

### Room flag

If the active task asks for a flag, retrieve it from the lab using the intended proof path and submit it to TryHackMe. The value is intentionally omitted here; use `flag{hidden}` in this public write-up.

## Phase 5: Detection

Useful signals include:
- Unexpectedly large or malformed session-cookie values in response headers.
- Multipart POST requests with unusually large bodies or excessive fields.
- Repeated upload-progress fields combined with memory-limit errors.
- PHP worker crashes, restarts, or out-of-memory errors.
- Unusual child processes or outbound connections from PHP workers.

No single indicator is conclusive. Correlate web access logs, PHP error logs, process telemetry, reverse-proxy logs, and network events. Preserve timestamps and affected worker information where available.

A PHP-based WAF may not be an effective primary control if the vulnerable code runs while the runtime parses the request, before the application-level filter processes it. Controls must be placed before the vulnerable parser or, preferably, the runtime must be patched.

## Phase 6: Remediation

The primary fix is to update PHP to a vendor-supported release that contains the security patches. The room's current reference identifies fixed releases as:

- PHP 8.5.12
- PHP 8.4.27
- PHP 8.3.36
- PHP 8.2.35

Verify the exact affected and fixed versions against the official PHP and vendor advisories before applying changes. Restart all relevant worker processes after patching so that old vulnerable runtime code is no longer resident.

Interim risk-reduction measures, where compatible with the application, include:
- Disable upload-progress tracking if it is not required.
- Restrict multipart upload requests at a network edge where possible.
- Run PHP with least privilege and limit filesystem/network access.
- Monitor PHP worker crashes and unexpected child processes.
- Review exposed PHP versions and restart workers after configuration changes.

These measures are not substitutes for patching. Blocking multipart requests may break legitimate uploads, and a PHP-level WAF may not inspect the request early enough to stop a runtime parser flaw.

## Phase 7: Reporting

A useful vulnerability report should include:
- Affected PHP version and deployment model.
- Whether the service is exposed to unauthenticated requests.
- Evidence of memory disclosure or code execution.
- The execution identity and demonstrated impact.
- Relevant request and error-log timestamps.
- The patch version and required worker restart.
- Temporary controls and their operational trade-offs.

Do not publish a weaponised exploit or expose an organisation's affected host details in a public report.

## Key takeaways

PHP2Shell demonstrates how a flaw in a language runtime can affect many applications that rely on it. The important concepts are memory safety, upload-progress handling, worker reuse, ASLR, and the difference between memory disclosure and code execution. Patch the runtime, restart workers, reduce process privileges, and correlate telemetry for signs of exploitation.