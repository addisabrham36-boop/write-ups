# OWASP Top 10 2025: Application Design Flaws

Official room: https://tryhackme.com/room/owasptopten2025two

## Room Overview & Information

**Category:** Web application security and secure design  
**Difficulty:** Beginner  
**Topics:** Security misconfiguration, software supply-chain failures, cryptographic failures, and insecure design.

This room groups vulnerabilities that often originate in how a system is built, configured, updated, or trusted. A vulnerability may exist even when the application code has no obvious memory bug: a key can be exposed, a dependency can be compromised, or a debug path can remain enabled in production.

## Task 1: Introduction and lab setup

Start the lab machine and use the TryHackMe AttackBox or a machine connected to the TryHackMe VPN. The practicals use ports on the target IP. Replace `MACHINE_IP` with the current address shown in the room; do not reuse another learner's address.

The categories covered in this room include:
- Security misconfiguration.
- Software supply-chain failures.
- Cryptographic failures.
- Insecure design.

## Task 2: Security misconfiguration

Security misconfiguration occurs when a system has unsafe defaults, exposed administrative functionality, excessive permissions, verbose errors, or incomplete hardening. The root cause is often deployment discipline: a test configuration or default secret survives into the running service.

The room's practical is available at:

```text
http://MACHINE_IP:5004
```

### Question: “What's the flag?”

1. Open the practical on port 5004.
2. Inspect the page and follow the application's clues.
3. Determine how the file is encrypted and where the application exposes information relevant to the key.
4. Preserve the original file and use the workflow provided by the practical to recover the key.
5. Decrypt the file and submit the flag displayed in the lab.

Do not assume the key is a default password or that a key from another version will work. The practical demonstrates why key material must not be exposed or derived from weak, public values. The exact flag is not reproduced here.

## Task 3: Software supply-chain failures

A supply-chain compromise targets a component or process that other systems trust. The vulnerable component may be a library, package, build pipeline, update server, model, or dataset. A trusted update mechanism can distribute malicious code widely if provenance and integrity are not checked.

The SolarWinds example is commonly used to explain how attackers can abuse a build and distribution pipeline rather than directly exploiting each downstream customer. The defensive questions are:
- Who produced the component?
- Was the source and build process trusted?
- Was the artifact signed and verified?
- Can the deployed version be traced to an approved build?
- Are dependencies pinned and monitored for known vulnerabilities?
- Can unusual runtime behaviour be detected?

Mitigations include signed artifacts, reproducible or controlled builds, protected CI/CD credentials, dependency inventory, version pinning, and verification before deployment.

## Task 4: Cryptographic failures

Cryptographic failures include weak algorithms, insecure modes, exposed keys, poor randomness, unsafe key derivation, and unencrypted sensitive data. Using a modern algorithm does not help if the secret key is hard-coded or stored beside the ciphertext.

Use established cryptographic libraries, authenticated encryption, secure random generation, and a key-management system. Never design a custom encryption algorithm for production. Rotate exposed keys and investigate whether previous ciphertext or credentials also need to be treated as compromised.

## Task 5: Insecure design

Insecure design is a failure to define and enforce security requirements for the intended workflow. Examples include a password-reset flow that can be abused, a payment approval process with no independent verification, or an AI agent that can perform high-impact actions without authorisation checks.

Secure design begins with abuse-case analysis and threat modelling. Identify assets, trust boundaries, attacker goals, and controls before implementation. Validate critical decisions in application code and require human approval for high-impact actions when appropriate.

## Phase 5: Verify and report

For each practical, record the vulnerable component, the observed behaviour, the evidence supporting the conclusion, the impact, and the mitigation. Do not confuse a suspected weakness with a verified result. Use the active room's output for the flag and redact it in a public write-up.

## Key takeaways

Application security depends on code, configuration, dependencies, cryptography, and design assumptions. Protect the entire lifecycle: verify components, keep secrets separate, deploy secure defaults, and test abuse cases before the system reaches production.