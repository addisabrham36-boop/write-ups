# No Scope: Finding RCE — TryHackMe Walkthrough

Room: https://tryhackme.com/room/noscope

Remote code execution (RCE) means an attacker can cause code or commands to run on a remote system. Finding it requires evidence that an input reaches an execution context in an unsafe way. A strange error, reflected string, or open port alone does not establish RCE.

## 1. Establish scope and collect baseline data

Start the room's machine and record its assigned address. Run a service scan and save the result:

    nmap -sC -sV -oN noscope-nmap.txt TARGET_IP

Use the actual address from the lab. Identify the exposed services and inspect the web application if HTTP or HTTPS is available. Keep the testing limited to the assigned target.

## 2. Map the application attack surface

Inspect pages, parameters, forms, file handling, API endpoints, and error responses. Record which inputs the server accepts and what happens to them. If a directory scan is required, use a wordlist available in the lab and manually review the results.

Prioritise features that process user-controlled values through interpreters or external utilities, but do not assume that any particular parameter is vulnerable before testing it.

## 3. Distinguish reflection from execution

A server may return user input in the response without executing it. RCE requires stronger evidence, such as a controlled benign expression being evaluated by the server or a harmless command producing a result that could only come from the target execution context.

Use the proof method required by the room. Do not run destructive commands, access unrelated secrets, or attempt to persist on the host. If the task provides an interactive shell, verify the host and user identity before continuing.

## 4. Analyse the input-to-execution path

Trace the input through validation, decoding, application logic, and any interpreter or subprocess. Identify whether the issue comes from command concatenation, unsafe deserialisation, a template engine, a vulnerable component, or another mechanism. The correct explanation depends on evidence from the target.

Document:
- The affected endpoint or feature.
- The user-controlled input.
- The processing step that causes unsafe interpretation.
- The minimal proof of execution.
- The privileges of the resulting process.
- Any assumptions or limitations.

## 5. Verify the impact and stop at the agreed boundary

RCE can range from execution as a low-privilege service account to broader compromise. Verify the actual context and avoid overstating the impact. A command running as the web-server user does not mean root access, and a successful proof does not justify exploring unrelated systems.

## 6. Remediation

Fix the unsafe input-to-execution path. Prefer safe library APIs over shell commands, use parameterised interfaces, apply strict input validation, patch affected components, and run the service with least privilege. Add regression tests that demonstrate the original input is handled as data rather than code.

Logging should capture security-relevant events while avoiding unnecessary storage of passwords, tokens, or private data. Detection can look for unexpected child processes, unusual request patterns, and web-server processes invoking shells.

## 7. Working through the room

Follow the active task and use the target's actual response to validate each hypothesis. If a test does not behave as expected, check the endpoint, encoding, request method, and lab state before trying alternatives. Do not publish a payload as confirmed unless it was tested in the room.

## What I took from this room

RCE is a claim that requires proof. The reliable approach is to map the attack surface, trace how input is processed, demonstrate the smallest safe execution result, and explain both the real impact and the underlying fix.