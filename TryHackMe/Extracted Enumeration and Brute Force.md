# Extracted: Enumeration and Brute Force — TryHackMe

Room: https://tryhackme.com/

I need to confirm the exact room page and challenge title before documenting the target-specific steps. The room list I was given describes it as “extracted enumeration and brute force,” which may be a shorthand rather than the official title.

## General workflow

Enumeration should come before guessing credentials. I should identify the exposed services, understand which authentication mechanism is in use, and check the room's clues for usernames or other relevant information. Brute-force or password-spraying tools should only be used when the lab explicitly authorises them and should be configured against the assigned target.

A generic Hydra pattern for an authorised lab is:

```bash
hydra -l USERNAME -P /path/to/wordlist.txt ssh://TARGET_IP
```

This is only an example for SSH and must not be assumed to match the room. The username, service, wordlist, and target must come from the actual exercise. Rate limits and lockout behaviour matter, and repeated guesses against real accounts can cause harm.

## What I took from this topic

The useful lesson is to gather evidence before attempting credentials and to understand the service being tested. I should only add the exact exploit path, command output, and task answers after checking the official room instructions.