# OWASP Top 10 2025: Insecure Data Handling

Official room: https://tryhackme.com/room/owasptopten2025three

## Room Overview & Information

**Category:** Web application security  
**Difficulty:** Beginner  
**Topics:** A04 Cryptographic Failures, A05 Injection, and A08 Software or Data Integrity Failures.

The room combines three common ways an application mishandles input or data: sensitive information is not adequately protected, user input is interpreted as instructions, or the application trusts data or software without verifying its integrity.

Start the lab machine and use the AttackBox or a machine connected to the TryHackMe VPN. Replace `MACHINE_IP` with the address shown by the active room.

## Task 1: Introduction

Read the task description and deploy the machine before attempting the practicals. The room has separate web applications on different ports, so make sure you use the port for the current task.

## Task 2: A04 Cryptographic Failures

Cryptographic failures occur when sensitive data is exposed because encryption is absent, weak, misused, or implemented with poor key management. Examples include plaintext passwords, outdated algorithms, weak derivation keys, exposed encryption keys, and missing transport encryption.

The practical is available at:

```text
http://MACHINE_IP:8001
```

It is a note-sharing service that uses a weak shared derivative key to protect notes.

### Question: “Decrypt the encrypted notes. One of them will contain a flag value. What is it?”

1. Open the note-sharing application on port 8001.
2. Follow the application's instructions to inspect and unlock the available notes.
3. Identify how the shared derivative key is used and what information the workflow exposes.
4. Decrypt the notes using the method demonstrated by the practical.
5. Locate the note containing the flag and submit the value shown by the lab.

Keep the original ciphertext and work on a copy if you export any artefact. The lesson is not to invent an encryption algorithm or treat a hidden key as secure merely because it is not shown on the page. Keys must be generated with appropriate entropy, protected separately from ciphertext, and rotated if exposed.

## Task 3: A05 Injection

Injection occurs when untrusted input is interpreted as a command, query, or template expression. Common examples include SQL injection, command injection, and Server-Side Template Injection (SSTI).

The practical is available at:

```text
http://MACHINE_IP:8000
```

The task demonstrates SSTI and asks you to read a file from the web application's directory.

### Question: “Perform an SSTI attack on the practical. You need to read the contents of flag.txt that is located within the same directory as the web application.”

1. Inspect the input field and learn how ordinary text is returned.
2. Use the task's guidance to determine whether the server evaluates template expressions.
3. Test the smallest harmless expression that confirms server-side evaluation.
4. Follow the room's intended workflow to read `flag.txt`.
5. Verify the response contains the file's content and submit the flag.

A string reflected back to the page is not enough to prove SSTI. Evidence of template evaluation is needed. The exact syntax depends on the engine used by the practical.

The fix is to use fixed templates and pass user input as data, restrict the web process's filesystem permissions, and avoid putting secrets in files accessible to the application.

## Task 4: A08 Software or Data Integrity Failures

Integrity failures occur when an application accepts software, updates, or data without adequate verification. A package may be altered, a deployment artifact may be replaced, or a workflow may trust input that should be authenticated.

When reviewing a software supply chain, verify the source, version, checksum or signature, build provenance, and approval status. A checksum only helps when the expected checksum comes from a trusted source. If both the artifact and checksum come from the same compromised location, the check provides little assurance.

Use signed updates, protected build pipelines, least-privilege CI/CD identities, dependency inventories, and monitoring for unexpected changes. Keep a record of which approved artifact was deployed to each environment.

## Phase 5: Reporting and verification

For each practical, record the vulnerable data flow, the observed evidence, and the actual impact. Submit values from the live lab rather than copying a flag from a different instance. In a public repository, redact the flag as `flag{hidden}`.

## Key takeaways

Cryptography protects data only when the algorithms, keys, and implementation are sound. Injection happens when data becomes executable syntax. Integrity failures occur when an application trusts artifacts or records without adequate verification. These problems need different controls, so identify the root cause before recommending a fix.