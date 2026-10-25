# OWASP Top 10 2025: Application Design Flaws — Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/owasptopten2025two

## Room Overview & Information

**Category:** Web application security  
**Difficulty:** Easy  
**Topics:** A02 Security Misconfiguration, A03 Software Supply Chain Failures, A04 Cryptographic Failures, and A06 Insecure Design.

This room contains four short practical tasks. Each task exposes a different class of weakness and returns a flag from a simulated application. It is not a single boot-to-root machine: use the individual web practicals, follow the relevant request flow, and submit the flag from each task. Do not confuse these categories with older OWASP Top 10 numbering; the official 2025 list labels them A02, A03, A04, and A06 respectively.

## Phase 1: Prepare the environment

Start the room's practical VM and use the AttackBox or a computer connected to the TryHackMe VPN. Some tasks use a web application served by the VM; always use the active IP displayed by the room. The flags below are the documented room values and may change if the exercise is updated.

For web tasks, capture a normal response before testing an invalid or unexpected input:

```bash
curl -i http://MACHINE_IP/
```

- `curl` sends an HTTP request.
- `-i` includes the response headers in the output.
- `MACHINE_IP` must be replaced with the current lab address.
- Use the task's actual port and path where specified.

The status code is one clue, not proof of a vulnerability. Read the body and correlate it with the task's required behaviour.

## Phase 2: Task-by-task walkthrough

## Task 1: Introduction

The task introduces four categories:

- **A02 Security Misconfiguration:** unsafe deployment settings, exposed debug interfaces, weak defaults, or unnecessary permissions.
- **A03 Software Supply Chain Failures:** compromised or unverified dependencies, libraries, updates, or build artifacts.
- **A04 Cryptographic Failures:** weak algorithms, predictable keys, insecure key handling, or sensitive data left unprotected.
- **A06 Insecure Design:** unsafe assumptions or missing security requirements built into a workflow or architecture.

**Question:** “I am ready to learn about design flaw vulnerabilities!”

**Answer:** No answer needed.

### Task 2: A02 Security Misconfigurations

A misconfiguration can leak internal state without a sophisticated exploit. Debug mode, verbose error pages, default credentials, and exposed administration endpoints all increase the attack surface.

Open the API practical at the address supplied by the room. In the documented instance, it listens on port 5002. Request a normal user record:

```bash
curl -i http://MACHINE_IP:5002/api/user/1
```

This establishes the expected response. The endpoint expects a numeric user identifier. Next, deliberately use an invalid identifier as the room instructs:

```bash
curl -i http://MACHINE_IP:5002/api/user/bad
```

Read the response body. In the vulnerable version, the error handler reveals internal debug information, including the flag. The failure is not simply that the input was invalid; it is that the application disclosed internal details to the client.

**Question:** “What's the flag?”

**Answer:** Copy the flag from the invalid-ID API response in the active room. Public copy: `THM{hidden}`.

**Remediation:** Validate request parameters, return a generic client-facing error, and keep stack traces and diagnostic data in access-controlled server logs. Disable debug mode in production and test the error-handling path with malformed inputs.

### Task 3: A03 Software Supply Chain Failures

Supply-chain failures occur when an application trusts a component, dependency, model, or update without adequately verifying its origin and integrity. A single compromised dependency can introduce code that the application team did not intentionally write.

Follow the room's software supply-chain practical. Inspect the artifact and the verification mechanism shown by the task. Determine whether the application proves who produced the artifact and whether its content has changed since approval. A filename or version label is not a cryptographic integrity check.

The intended flag in the documented room is:

```text
THM{SUPPLY_CH41N_VULN3R4B1L1TY}
```

**Question:** “What's the flag?”

**Answer:** Copy the flag shown by the active supply-chain practical. Public copy: `THM{hidden}`.

**Evidence to record:** the component or artifact involved, the missing verification step, and the point where the system accepts an untrusted or tampered dependency. Do not assume every outdated library is compromised; the actual flaw is the failure to control provenance or integrity.

**Remediation:** Use trusted registries, pin dependency versions, review release changes, verify signatures or hashes from an authenticated source, protect CI/CD credentials, and maintain an inventory of software components. Hashes only establish integrity when compared against a trusted reference; an attacker who can replace both the artifact and an unauthenticated checksum can defeat that check.

### Task 4: A04 Cryptographic Failures

Cryptographic failures include weak keys, hard-coded secrets, obsolete algorithms, incorrect protocol use, and insufficient protection for sensitive data. Cryptography should use well-reviewed implementations rather than custom schemes.

Follow the room's cryptographic practical and identify the key-handling mistake described by the exercise. In the documented task, the flag is:

```text
THM{CRYPTO_FAILURE_H4RDCOD3D_K3Y}
```

**Question:** “What's the flag?”

**Answer:** Copy the flag shown by the active cryptographic-failure practical. Public copy: `THM{hidden}`.

**Why it matters:** A hard-coded key may be recoverable from source, a binary, configuration, or version-control history. If every installation uses the same key, a single disclosure can compromise multiple deployments.

**Remediation:** Use a secret manager or protected key store, generate keys with sufficient entropy, limit who can read them, rotate exposed keys, and use authenticated encryption through a reputable library. Passwords should be stored using a dedicated password-hashing scheme such as Argon2id, scrypt, or bcrypt, not reversible encryption or a fast general-purpose hash.

### Task 5: A06 Insecure Design

Insecure design exists when a workflow or architecture lacks the security controls it needs. Unlike a simple coding defect, the flaw may be present even when the software accurately implements the original design. Examples include account recovery without identity proof, allowing the same user to create and approve a sensitive transaction, or granting an AI agent authority without independent policy checks.

Read the scenario and identify the assumption that makes the application unsafe. The intended flag in the documented task is:

```text
THM{1NS3CUR3_D35IGN_4SSUMPT10N}
```

**Question:** “What's the flag?”

**Answer:** Copy the flag shown by the active insecure-design practical. Public copy: `THM{hidden}`.

**Remediation:** Create abuse cases during design, define security requirements before implementation, enforce authorization server-side, use independent approval for high-risk actions, and retest the full workflow. AI features need the same controls: do not allow model output alone to authorize payments, expose private data, run commands, or change records.

### Task 6: Conclusion

**Question:** “I'm ready for the next room!”

**Answer:** No answer needed.

## Phase 3: Consolidated question and flag table

| Task | Question | Answer |
|---|---|---|
| Task 1: Introduction | I am ready to learn about design flaw vulnerabilities! | No answer needed |
| Task 2: A02 Security Misconfigurations | What's the flag? | Obtain from active task; redacted as `THM{hidden}` |
| Task 3: A03 Software Supply Chain Failures | What's the flag? | Obtain from active task; redacted as `THM{hidden}` |
| Task 4: A04 Cryptographic Failures | What's the flag? | Obtain from active task; redacted as `THM{hidden}` |
| Task 5: A06 Insecure Design | What's the flag? | Obtain from active task; redacted as `THM{hidden}` |
| Task 6: Conclusion | I'm ready for the next room! | No answer needed |

## Phase 4: Key Takeaways & Remediation

1. **Configuration:** debug details belong in protected logs, not public error responses.
2. **Supply chain:** verify source and integrity before installing or executing artifacts.
3. **Cryptography:** use standard algorithms and secure key management; never embed reusable secrets in source code.
4. **Design:** identify assets, trust boundaries, abuse cases, and authorization requirements before implementation.
5. **Verification:** test normal and invalid workflows, capture evidence, and confirm that a fix blocks the actual root cause.

Security needs to be designed into the application and verified at deployment. A final layer of filters cannot compensate for a system that trusts an unverified dependency, leaks its internal errors, mishandles keys, or gives an unsafe workflow too much authority.
