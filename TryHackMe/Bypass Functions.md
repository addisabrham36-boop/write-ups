# Bypass Functions: TryHackMe Walkthrough

Room: https://tryhackme.com/room/bypassfunctions

## Room Overview & Information

**Category:** Web application security / PHP hardening  
**Focus:** Understanding restricted PHP functions, why blacklists fail, tracing input into an interpreter, and proving a weakness safely.

The room title suggests a function restriction or filtering challenge. The correct bypass depends on the exact PHP version, disabled-function configuration, available extensions, and how the application uses user input. Do not infer a specific exploit from the title alone. The walkthrough below sets out a reproducible way to analyse the actual lab without inventing a payload or claiming a result that has not been observed.

## Phase 1: Start the lab and inspect the application

Start the target and record the assigned IP. Identify the web service:

\`\`\`bash
nmap -sC -sV -oN bypass-functions-nmap.txt TARGET_IP
\`\`\`

- **-sC** runs Nmap's default scripts.
- **-sV** attempts to identify service versions.
- **-oN** saves the output for review.

Open the web page and identify the input field or function described by the task. Record the request method, endpoint, parameters, response status, and error text. A field that accepts input is not automatically vulnerable; the key question is how the server processes the value.

## Phase 2: Understand PHP function restrictions

PHP's configuration can disable selected functions. Administrators sometimes use the disabled-functions setting to reduce the risk of dangerous operations such as process execution. This can be useful as defence in depth, but it is not a complete sandbox. The application may still expose equivalent functionality through another API, extension, or unsafe data flow.

The exact available functions depend on the installed PHP version and extensions. If the room exposes configuration information, inspect only the fields required by the task. Do not assume a function is disabled merely because a website returns an error.

A secure design should avoid allowing untrusted input to control an interpreter or command in the first place. A blacklist of function names is not a substitute for separating data from executable instructions.

## Phase 3: Trace the input path

Use the browser's Network tab or Burp Suite to capture the request. Send it to Repeater and change one input at a time.

For each test, record:
1. The original input.
2. The expected behaviour.
3. The actual response.
4. Any error or transformation.
5. Whether the server evaluated the input or simply displayed it.
6. The evidence that supports the conclusion.

If the application evaluates a value, identify the language and context. PHP string parsing, shell parsing, SQL syntax, and template syntax have different rules. A character that is harmless in one context may be meaningful in another.

## Phase 4: Determine what the filter actually blocks

A filter may reject a function name, a character, a keyword, or a pattern. Test the rule with harmless inputs that reveal its behaviour. Do not begin by trying a long list of obfuscated payloads.

Check whether the application:
- Rejects the input before PHP processes it.
- Rewrites or removes part of the input.
- Passes the value to a PHP function.
- Passes the value to an operating-system command.
- Uses the value in a database query.
- Displays the value without executing it.

This distinction determines what a successful bypass would mean. A filter bypass is not proof of code execution unless the server actually performs the unsafe operation.

## Phase 5: Validate impact safely

Use only the proof method provided by the active lab. Prefer a harmless output that confirms the relevant behaviour without changing system state. If the task demonstrates command execution, verify the execution context and record the result.

Do not use destructive file operations, persistence, credential theft, or commands against systems outside the room. If a shell is part of the lab, confirm the identity and host before any further action. Code execution under a web-server account does not automatically mean root access.

## Phase 6: Troubleshooting

- **Input is rejected:** inspect the exact error and determine which rule triggered it.
- **Input is reflected but not evaluated:** reflection alone is not code execution.
- **A test works locally but not remotely:** the PHP version, configuration, extension set, or application path may differ.
- **A function appears disabled:** verify the actual configuration and whether an alternate code path is available.
- **Output is inconsistent:** change one variable at a time and preserve each request/response pair.

## Phase 7: Remediation

- Avoid shell execution for operations that can be performed with safe library APIs.
- If a subprocess is required, pass a fixed executable and structured arguments rather than concatenating user input.
- Validate inputs against a strict allowlist appropriate to the expected data.
- Use parameterised queries and context-aware output encoding.
- Run the web process with minimal permissions.
- Keep PHP and extensions patched.
- Treat disabled-functions settings as supplementary hardening, not the primary security boundary.
- Add regression tests for equivalent syntax and alternate input representations.

## Phase 8: Complete the room using live evidence

Use the task's exact input and output. Identify the restriction, test it methodically, and follow the room's intended proof. If a question requests a particular value or flag, obtain it from the current lab and redact the flag in public notes. This write-up deliberately does not invent an exploit string or claim that a specific bypass succeeded without evidence from the active target.

## Key takeaways

A meaningful bypass crosses a defined security boundary. Understand the input path, the interpreter, and the restriction being applied. The durable fix is to remove the unsafe execution path, validate input, and reduce the permissions of the process that handles requests.

## Expanded technical workflow: restricted-function bypass

### Establish the intended boundary

Read the task brief and identify the exact function or command the lab says is blocked. Record the input field, expected response, and the restrictions the application claims to enforce. Test only the supplied challenge instance.

The central question is whether the control blocks a capability securely or merely rejects a small list of strings. A blacklist can be bypassed by alternate syntax, encodings, aliases, or an unexpected execution path. Do not assume a bypass exists until a harmless test demonstrates that the restricted behaviour actually occurs.

### Observe before changing inputs

Capture a baseline request and response. In a browser, use Developer Tools; in an HTTP challenge, an intercepting proxy can preserve the method, path, headers, cookies, and body. Change one element at a time and record the result. This makes it possible to distinguish server validation from client-side validation and from downstream command execution.

If the task is about command execution, prefer harmless inspection commands permitted by the room. Never test payloads against a production service or a host outside the lab.

### Understand the parsing layers

A value may pass through multiple interpreters: URL decoding, application validation, shell parsing, a template engine, and the operating system. A filter that runs before decoding may see a different string from the component that eventually executes it. Conversely, special characters may be escaped or removed before reaching the sensitive function.

Document which layer appears to interpret the value. A server error alone does not prove command execution; confirm the effect using a safe, observable result specified by the task.

### Validate a suspected bypass

A sound test includes:
1. The original input and baseline response.
2. The modified input and exact response.
3. A clear explanation of which validation rule was evaded.
4. Evidence that the intended restricted operation occurred.
5. A negative control, where possible, showing the baseline behaviour differs.

Do not treat an HTTP 200 response as proof of success. Inspect the response body, application state, or other explicit evidence.

### Defensive recommendations

Avoid denylist-only filtering. Do not construct shell commands from untrusted input. Use fixed functions and structured arguments, allowlist expected values, apply least privilege, isolate the process, and validate authorization on the server. If shell use is unavoidable, use safe APIs and avoid passing user-controlled text through a shell interpreter. Log rejected inputs in a way that does not expose secrets.

### Troubleshooting and room answers

If the input appears to have no effect, confirm that you are editing the correct parameter, that the request reaches the intended endpoint, and that the lab instance is active. Check whether the application normalizes input before validation. Use the room's own task feedback to determine the requested answer, and redact any flag as `flag{hidden}` in public notes.
