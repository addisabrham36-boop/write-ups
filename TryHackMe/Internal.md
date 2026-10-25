# Internal — TryHackMe

Room: https://tryhackme.com/room/internal

Internal is a machine-based penetration-testing room. I should use the IP assigned by the room and build the route from the evidence I collect, rather than copy a target address or assume another person's scan matches my instance.

## Initial enumeration

I start with a service scan and save the output:

```bash
nmap -sV -sC -oN internal-scan.txt TARGET_IP
```

I then inspect the services actually reported. If a web application is exposed, I browse it, check available paths, and note any authentication or content clues. Enumeration tools can help find files and routes, but I should keep their scans within the room's target and rate limits.

## Investigate, do not assume

A discovered login page does not prove that credentials are weak. A version banner does not prove that an exploit works. I should use each clue to choose the next test, then compare the result with what I expected.

If I gain access during the lab, I check the current user and permissions before investigating escalation. I should understand the commands I run and avoid executing random scripts with elevated privileges.

## What I took from this room

Internal reinforces the usual workflow: discover, enumerate, validate, and document. A useful walkthrough explains how each observation led to the next step and redacts any actual flag as `flag{hidden}`, rather than publishing secrets.