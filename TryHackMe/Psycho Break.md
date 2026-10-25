# Psycho Break — TryHackMe

Room: https://tryhackme.com/room/psychobreak

Psycho Break is a themed machine room. I should treat the theme as atmosphere and still work through the target methodically: identify exposed services, inspect the web application if present, and use clues from the actual instance to decide what to test.

## Start with a scan

After launching the room and obtaining its target IP, I can run:

```bash
nmap -sV -sC -oN psycho-break-scan.txt TARGET_IP
```

This is a general initial scan, not a claim about the room's current ports. I should read the output and investigate only the services that are actually present.

## Use clues, not guesses

For web content, I inspect pages, links, source, and any files the challenge explicitly exposes. If the room provides a file or puzzle clue, I record what it contains and how it points to the next step. Randomly trying payloads without understanding the evidence makes the process harder to reproduce.

If the room leads to shell access, I check the current user and permissions, then investigate the intended escalation path. Any commands or findings in a final walkthrough should match the actual lab run.

## What I took from this room

A themed machine is still a technical investigation. The reliable workflow is enumeration, clue analysis, controlled validation, and clear notes. I should add the exact exploit path and redacted flag locations after verifying them against the running target.