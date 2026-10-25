# Gaming Server — TryHackMe

Room: https://tryhackme.com/room/gamingserver

Gaming Server is a machine-based lab, so I should use the IP address assigned by the room and follow the evidence from the running target. A walkthrough copied from another session may refer to a different address or target state.

## Initial enumeration

I begin by checking the exposed services and saving the results:

```bash
nmap -sV -sC -oN gaming-server-scan.txt TARGET_IP
```

This is a general first scan. I then inspect the actual output and investigate the services that are present. If HTTP is exposed, I browse the site and examine its content, links, and any files the challenge makes available.

## Follow the clues

I should inspect files before using them and avoid assuming that a clue means a particular credential or exploit. If the room exposes a repository, upload feature, or authentication endpoint, I investigate its behaviour using the allowed lab steps. Each conclusion should be supported by what I observe.

If the challenge leads to shell access, I check the current user and permissions before exploring further. Public notes should redact actual flags as `flag{hidden}` while explaining the steps that revealed them.

## What I took from this room

Enumeration is most useful when each result determines the next step. I should save scan output, investigate real clues, and avoid making claims about commands or flags that I have not verified in the active lab.