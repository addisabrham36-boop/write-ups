# OWASP Top 10 2025: Security Misconfiguration

Room: https://tryhackme.com/room/owasptopten2025two

Security misconfiguration happens when a system is deployed with unsafe settings, unnecessary exposure, weak defaults, or incomplete security controls. The official room groups this practical with other design-flaw topics, including software supply-chain failures, cryptographic failures, and insecure design.

## 1. Recognise misconfiguration

Examples include:
- Default credentials that were never changed.
- Debug or administrative interfaces exposed publicly.
- Unnecessary services and ports left enabled.
- Incorrect permissions on files, directories, or cloud resources.
- Weak TLS configuration or invalid certificate handling.
- Secrets stored in source code, logs, or public configuration files.
- Insecure defaults in AI or model-serving infrastructure.

A setting is not automatically a vulnerability simply because it differs from a personal preference. It becomes a security issue when it violates a requirement or creates an exploitable exposure.

## 2. Work through the practical

Start the lab machine and use the address assigned by TryHackMe. The room's security-misconfiguration practical is available on port 5004 of the target. Open the page at the form http://MACHINE_IP:5004, replacing MACHINE_IP with the current lab address.

The task asks you to find the key needed to decrypt a file. Inspect the application and its provided clues carefully. Look for exposed configuration, weak key handling, or information that the application reveals unintentionally. Do not use a key from a different instance; the exact value must come from the active lab.

If the application provides a file and a documented decryption process, preserve the original and work on a copy. Identify the file format and the algorithm or script used by the lab before trying tools. A key must match the algorithm and data format; guessing random values is not a reliable method.

## 3. Understand the cryptographic lesson

Cryptographic failures and misconfiguration often overlap. Strong algorithms can still be undermined by hard-coded keys, weak key derivation, unsafe modes, or keys stored alongside the encrypted data. Do not invent a custom encryption scheme for production systems.

Use established cryptographic libraries, authenticated encryption where appropriate, and a secure key-management process. Keep keys separate from ciphertext, restrict who can access them, and rotate them according to policy.

## 4. Check the deployment

Review which services are reachable, which accounts can access management interfaces, whether debug mode is enabled, and whether permissions are more generous than necessary. Compare the running configuration with a documented baseline. Remove unused components and make secure settings repeatable through configuration management.

## 5. Verify the fix

A remediation should be testable. If the issue was an exposed debug page, verify it is no longer reachable by an unauthorised user. If the problem was an exposed key, remove the exposure, rotate the key, and check logs or repository history for additional copies. If weak permissions caused the issue, test with a lower-privileged account.

## 6. Working through the room

Use the exact practical URL and file supplied by the room. Follow the clues to identify the key and decrypt the intended file, then submit the flag shown by the lab. The public note does not include an unverified flag value.

## What I took from this room

Misconfiguration is often a failure of deployment discipline rather than a sophisticated exploit. Secure defaults, restricted access, sound key management, and repeatable configuration reviews prevent many avoidable exposures.