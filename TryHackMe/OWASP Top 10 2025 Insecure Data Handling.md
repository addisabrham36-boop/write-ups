# OWASP Top 10 2025: Insecure Data Handling — TryHackMe Walkthrough

Official room: https://tryhackme.com/room/owasptopten2025three

## Room Overview & Information

**Category:** Web application security  
**Difficulty:** Easy  
**OWASP categories:** A04 Cryptographic Failures, A05 Injection, and A08 Software or Data Integrity Failures.

The room contains three practicals: decrypting a note protected by a weak shared key, abusing server-side template rendering to read a lab file, and demonstrating unsafe deserialization. These tasks show three different root causes and should not be treated as one generic “input validation” problem.

Start the machine and use the AttackBox or a machine connected to the TryHackMe VPN. Use the active room's IP address and keep all testing inside the lab. Flags are redacted in this public write-up.

## Task 1: Introduction

The room covers three categories:
- **A04 Cryptographic Failures:** sensitive data is not adequately protected.
- **A05 Injection:** untrusted input is interpreted as instructions or expressions.
- **A08 Software or Data Integrity Failures:** an application trusts data or artifacts without adequately verifying their integrity.

## Task 2: A04 Cryptographic Failures

### What the practical demonstrates

The note-sharing service encrypts notes using a weak, shared derivative key. A key with a tiny search space is not safe merely because the ciphertext looks random. The application also exposes enough information for the learner to infer the key.

Open the practical at:

```text
http://MACHINE_IP:8001
```

Replace MACHINE_IP with the address assigned by the active room.

### Question: “Decrypt the encrypted notes. One of them will contain a flag value. What is it?”

The room hints that the key is four bytes and begins with the letters KEY. The intended key is:

```text
KEY1
```

Enter this key in the note-sharing interface and select **Decrypt All Notes**. Inspect the decrypted notes and copy the flag from the one containing it. The flag is redacted as `THM{hidden}`.

### Why this is a cryptographic failure

A short predictable key can be guessed or searched quickly. A shared key also creates a broad impact if it is exposed. Use established authenticated-encryption schemes, generate keys with sufficient entropy, and store them in a server-side secret manager rather than exposing them to every client.

## Task 3: A05 Injection

### What the practical demonstrates

The application renders dynamic content using a server-side template engine. Server-Side Template Injection (SSTI) occurs when user-controlled input is interpreted as template syntax rather than treated as plain text. Depending on the engine and its permissions, SSTI may expose configuration, read files, or execute commands.

Open the practical at:

```text
http://MACHINE_IP:8000
```

Replace MACHINE_IP with the active lab address.

First, use the task's harmless test to establish that the server evaluates template expressions. A reflected string alone is not proof of SSTI; the output must show that the server interpreted the expression.

### Question: “What's the flag?”

The task's intended path is to use the vulnerable template context to read the contents of `flag.txt` in the application's directory. Follow the room's supplied hints and verify that the response contains the file's contents rather than merely echoing your input. Submit the flag shown by the active lab; it is redacted here as `THM{hidden}`.

### Why this is an injection vulnerability

The application has allowed untrusted input to cross into the template interpreter. The safe design is to use fixed templates and pass user input as data. Do not evaluate user-supplied template syntax. Also run the application with limited filesystem permissions so a template bug cannot access unrelated secrets.

## Task 4: A08 Software or Data Integrity Failures

### What the practical demonstrates

Unsafe deserialization occurs when an application loads attacker-controlled serialized data without validating its source and structure. In Python, pickle is especially sensitive because deserializing a crafted object can invoke callable behaviour. Pickle should not be used to load untrusted data.

The practical asks you to create a serialized object that causes the application to read `flag.txt`. Follow the room's instructions and use only its test endpoint and local flag file. The critical concept is that the deserializer trusts the incoming object enough to invoke behaviour while loading it.

### Question: “What are the contents of flag.txt?”

1. Inspect the application's request format and identify where the serialized object is accepted.
2. Follow the task's guidance to construct the expected serialized test object in the lab.
3. Encode the serialized bytes in the format the endpoint expects.
4. Submit the request to the room's endpoint.
5. Verify the response contains the contents of the lab's `flag.txt`.
6. Submit the flag shown by the active instance.

The flag is redacted here as `THM{hidden}`. This write-up intentionally does not include a copy-paste malicious pickle payload; the security lesson is that untrusted serialized data must never be deserialized with a mechanism that can invoke arbitrary callables.

### Why this is an integrity failure

The application treats a user-supplied artifact as trusted executable object data. Safer designs use non-executable data formats such as JSON with strict schema validation, authenticate artifacts where appropriate, and verify their provenance before processing them.

## Phase 5: Reporting and Remediation

### Cryptographic failures
- Replace short or predictable keys with securely generated keys.
- Use authenticated encryption with correct nonce handling.
- Keep keys on the server and protect them with proper key management.
- Rotate any key that has been exposed.

### SSTI
- Do not evaluate user-controlled template strings.
- Render untrusted values as data.
- Keep the web process least-privileged.
- Restrict file and network access available to the renderer.

### Unsafe deserialization
- Do not deserialize untrusted pickle data.
- Prefer a non-executable data format with strict schema validation.
- Authenticate and verify the integrity of trusted artifacts.
- Isolate processing of untrusted inputs and log unexpected failures.

## Key takeaways

The three practicals have different root causes. A predictable encryption key weakens confidentiality; SSTI lets input influence a template interpreter; unsafe deserialization lets untrusted object data trigger behaviour. Identify the specific trust boundary, verify the impact in the lab, and apply a fix that removes the root cause rather than relying on a generic filter.

## Expanded data-handling assessment

### 1. Trace data through the application

Identify where data enters, is validated, stored, transformed, logged, transmitted, and rendered. At every boundary, ask whether the system knows the data's source, whether it is trusted, and how it is encoded for the next component. The same data may be safe in one context and dangerous in another.

### 2. Inspect validation and parsing

Check whether the server validates the expected type, size, format, and allowed values. Client-side validation improves usability but can be bypassed. Parsers for archives, images, documents, and serialized objects deserve special attention because they may consume complex attacker-controlled structures.

### 3. Compare raw and processed values

Capture the original request, processed output, and any error returned by the lab. Determine whether the application decodes or normalizes data before validation and whether later components interpret it differently. Do not assume every malformed value is exploitable; confirm impact through a safe, repeatable test.

### 4. Protect secrets and personal data

Avoid placing credentials, tokens, or sensitive records in URLs, client-side assets, debug responses, or logs. Minimize collection and retention, control access, and encrypt data in transit and at rest where appropriate. Encryption does not replace authorization or safe application logic.

### 5. Remediation

Use schema validation, safe parsers, strict content-type handling, size and timeout limits, context-appropriate output encoding, secret-management services, and least-privilege storage access. Treat file uploads and external API responses as untrusted input.

### 6. Room questions

Connect each task to the affected data flow, validation step, parser, or trust boundary. Confirm the exact result in the lab and do not invent sample values. Redact flags as `flag{hidden}`.
