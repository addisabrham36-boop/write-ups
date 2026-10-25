# OWASP Top 10 2025: Injection

Official room: https://tryhackme.com/room/owasptopten2025three

## Room Overview & Information

**Category:** Web application security  
**Difficulty:** Beginner  
**OWASP category:** A05 Injection  
**Practical:** Server-Side Template Injection (SSTI) against the room's deliberately vulnerable web application.

The official room is titled **OWASP Top 10 2025: Insecure Data Handling** and also covers cryptographic failures and software/data integrity failures. This file focuses on the injection task so the vulnerability and its practical steps are easy to find.

## Phase 1: Understand the vulnerability

Injection occurs when untrusted input is interpreted as part of a command, query, template, or other language. The application has accidentally allowed data to influence program structure.

Examples include SQL injection, operating-system command injection, and Server-Side Template Injection (SSTI). The interpreter differs, but the core question is the same: did user-controlled data remain data, or was it evaluated as instructions?

## Phase 2: Start the practical

Deploy the lab machine. Use the TryHackMe AttackBox or a testing machine connected to the TryHackMe VPN. The SSTI practical is available at:

```text
http://MACHINE_IP:8000
```

Replace `MACHINE_IP` with the current address shown by the room. Do not use a value from an old instance.

Inspect the web form and identify the field that controls dynamic content. Submit ordinary input first so you understand the normal response. Then use the room's instructions to test whether the server evaluates a harmless template expression.

## Phase 3: Confirm SSTI instead of ordinary reflection

A reflected string is simply input echoed back to the browser. SSTI occurs when a server-side template engine evaluates user-controlled template syntax.

A sound test compares:
1. Ordinary text, which should be displayed as text.
2. A harmless expression supported by the template engine, which may be evaluated.
3. The response, to establish whether the server performed evaluation.

The exact syntax depends on the template engine. Do not assume that an expression from Jinja, Twig, or another engine works in every framework. Follow the active task's hint and keep testing within the lab.

## Phase 4: Retrieve the requested file

### Task question

**“Perform an SSTI attack on the practical. You need to read the contents of flag.txt that is located within the same directory as the web application.”**

Procedure:
1. Identify the template behaviour using the room's supplied application.
2. Follow the task's intended SSTI method to access a file in the application's directory.
3. Read the contents of `flag.txt` as instructed by the lab.
4. Verify that the returned value is the file content, not a string echoed from your input.
5. Submit the flag to TryHackMe.

This public note does not include a guessed flag. The exact value should come from the active practical, and in a public repository it should be redacted as `flag{hidden}`.

## Phase 5: Evidence and impact

Record the endpoint, the input that caused template evaluation, the resulting response, and the privileges of the application process. Access to one file does not automatically mean full server compromise. State only the impact the lab demonstrates.

SSTI can be severe because template engines may expose application objects, configuration, or file access depending on the engine and environment. The impact varies by framework, sandboxing, and process permissions.

## Phase 6: Remediation

- Do not compile or evaluate user-controlled strings as templates.
- Use fixed templates and pass user input as data.
- Apply framework-supported sandboxing where appropriate, while recognising that sandboxing is not a substitute for safe design.
- Run the web process with least privilege and restrict filesystem access.
- Avoid storing secrets in files accessible to the web process.
- Add regression tests that ensure user input is rendered literally.
- Keep frameworks and template engines patched.

Escaping must be context-aware. HTML output encoding does not automatically protect SQL, shell, or template contexts.

## Key takeaways

SSTI is an injection vulnerability in which input crosses into a server-side template interpreter. The lab's objective is to prove evaluation and retrieve the specified file. The durable fix is to keep user input separate from executable template syntax and minimise the application's permissions.

## Expanded injection testing methodology

### 1. Identify the interpreter

Injection occurs when untrusted data is interpreted as part of a command or query. Identify the sink: SQL engine, operating-system shell, template renderer, LDAP query, or another interpreter. The correct test and remediation depend on the sink.

### 2. Establish a baseline

Capture a normal request and response from the room. Identify exactly which input is under test and how it reaches the backend. Change one input at a time and compare response content, errors, timing, and state changes. Do not infer injection from an error string alone.

### 3. Use controlled validation

Follow the room's prescribed benign payloads. Confirm that the result is caused by the input and is repeatable. For database queries, a difference in returned rows or a controlled boolean condition may be relevant; for shell or template injection, use only the harmless proof specified by the lab. Do not use destructive commands or test unrelated systems.

### 4. Find the root cause

The central failure is usually mixing data with executable syntax or relying on incomplete filtering. Encodings and alternate syntax may expose a filter that checks the wrong representation, but the durable fix is not to add more blacklist entries.

### 5. Remediation

Use parameterized SQL statements, safe APIs, structured arguments, context-aware output encoding, and least-privilege service accounts. Avoid shell interpretation for user-controlled values. Where templates support raw expressions, restrict the features and run rendering in an isolated environment.

### 6. Record room answers accurately

For every task, preserve the input, output, affected sink, and demonstrated effect. Use the live room to obtain exact values; do not fabricate payload output. Redact public flags as `flag{hidden}`.
