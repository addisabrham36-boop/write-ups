# TryHack3M: Bricks Heist — TryHackMe

Room: https://tryhackme.com/room/tryhack3mbricksheist

This room focuses on investigating a web environment and identifying evidence related to the scenario. I should begin with the exact target information from the room and avoid reusing another person's IP address or assumed output.

## Enumeration

For the authorised target, I can save an initial service scan:

```bash
nmap -sV -sC -oN bricks-heist-scan.txt TARGET_IP
```

Then I inspect the reported services and open the website if HTTP or HTTPS is available. I should pay attention to page content, headers, exposed paths, and the software or framework clues that are actually present.

## Validate the clues

A detected technology or version is a lead, not proof of a vulnerability. I should verify that a suspected issue applies to the target and follow the challenge's intended steps. If logs or other artefacts are part of the room, I should examine their timestamps and content rather than infer the answer from a filename alone.

## What I took from this room

A web investigation should move from discovery to evidence-based validation. I need to keep the target in scope, save useful output, and document what the lab actually demonstrates. Any real flag should be replaced by `flag{hidden}` in a public walkthrough.