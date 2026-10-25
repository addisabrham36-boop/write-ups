# Road — TryHackMe

Room: https://tryhackme.com/room/road

Road is a machine-based practice room, so the correct walkthrough depends on the target instance and the evidence exposed during the session. I should not copy an IP address or assume the same target state as another person's run.

## Enumeration

I begin with the IP address assigned by the room and save a service scan:

```bash
nmap -sV -sC -oN road-scan.txt TARGET_IP
```

This attempts service-version detection, runs Nmap's default scripts, and stores the output. From there, I inspect the services actually reported and follow the room's clues. If a web service is present, I browse it and record the available routes and behaviour before trying anything more intrusive.

## Follow the evidence

For each finding, I ask what it tells me and what it does not tell me. A login page does not prove that credentials are weak, and an old-looking banner does not prove a vulnerability is exploitable. I should test only the weaknesses that fit the observed service and stay within the room's scope.

## Privilege escalation and reporting

If the lab leads to shell access, I need to understand the account and permissions before investigating escalation paths. Commands that enumerate identity, groups, processes, and file permissions can provide context, but I should avoid running arbitrary scripts without reviewing what they do.

Any room flags should be redacted as `flag{hidden}` in a public write-up, with the discovery step explained rather than the actual secret.

## What I took from this room

Machine-based pentesting works best as a chain of evidence: enumerate, investigate, validate, and then document. I should add the exact route and results from my own target session before treating this as a completed exploit walkthrough.