# OWASP Top 10 2025: Injection

Room: https://tryhackme.com/room/owasptopten2025three

The official OWASP Top 10 2025 room titled Insecure Data Handling covers cryptographic failures, injection, and software or data integrity failures. This note focuses on injection, where untrusted input is interpreted as part of a command, query, template, or other executable language instead of being handled as ordinary data.

## 1. Understand the vulnerability

Common forms include SQL injection, operating-system command injection, and server-side template injection (SSTI). They differ in the interpreter involved, but the underlying mistake is similar: user-controlled input reaches a parser in a way that changes the intended program structure.

For SQL, parameterised queries keep values separate from query syntax. For operating-system commands, avoid invoking a shell with concatenated user input. For templates, do not treat user-supplied text as a trusted template expression.

## 2. Start the practical

Deploy the room's lab machine and use the AttackBox or a machine connected to the TryHackMe VPN. The injection practical is available at port 8000 of the target. Open http://MACHINE_IP:8000, replacing MACHINE_IP with the address shown for your instance.

The task demonstrates SSTI in a vulnerable application. It asks you to retrieve the contents of flag.txt located in the same directory as the web application. Read the task instructions and inspect how the application renders the input you control.

## 3. Identify server-side template evaluation

A normal template engine replaces placeholders with values according to its syntax. SSTI occurs when user-controlled input is interpreted as template code rather than inserted as plain text. A useful test distinguishes ordinary reflection from actual evaluation. A page echoing your input does not prove SSTI; evidence that the server evaluates a harmless expression is stronger.

Use only the supplied lab. Test the smallest harmless expression needed to identify the template behaviour, then follow the room's intended steps to access the requested file. The exact syntax depends on the template engine, so do not assume one expression works across all frameworks.

## 4. Verify the result

Record the input, response, and evidence that the server evaluated it. If the room asks for the file's flag, obtain it from the live practical and submit it there. This public note does not include a guessed flag or a payload claimed to have been tested in an instance I did not run.

## 5. Prevent injection

- Keep untrusted input separate from executable syntax.
- Use parameterised SQL queries.
- Avoid shell invocation where a safe API is available.
- Do not render user-controlled strings as trusted templates.
- Use strict input validation appropriate to the expected data.
- Run the application with least privilege and restrict filesystem access.
- Keep frameworks patched and add regression tests for the vulnerable input path.

Escaping rules differ between contexts. A filter that is safe for HTML may not be safe for SQL or a shell. Use the protection designed for the specific interpreter.

## What I took from this room

Injection happens when data crosses into a language interpreter without the right boundary. The reliable fix is to use safe APIs and context-specific protections, then verify that the original input is treated as data rather than executable syntax.