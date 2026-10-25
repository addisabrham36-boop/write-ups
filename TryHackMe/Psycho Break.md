# Psycho Break: TryHackMe Walkthrough

Room: https://tryhackme.com/room/psychobreak

Psycho Break is a themed CTF challenge. The story is useful for atmosphere, but the solution still depends on evidence from the target: exposed services, files, application behaviour, and the permissions available after access. This note gives a repeatable process without inventing target-specific credentials or flags.

## 1. Start the target and enumerate

Launch the room machine and record its assigned IP. Start with a service scan:

    nmap -sC -sV -oN psychobreak-nmap.txt TARGET_IP

If the initial scan does not explain the exposed surface, scan all TCP ports:

    nmap -p- --min-rate 1000 -oN psychobreak-allports.txt TARGET_IP

Replace TARGET_IP with the current address. Review the ports and versions, then investigate the services that actually respond. Allow the machine time to boot before treating a connection failure as meaningful.

## 2. Inspect the web application and files

If a web service is present, open it in a browser and record the page content, links, source, and any unusual paths. Check robots.txt and other clues referenced by the application. If the room supplies files, use file and sha256sum to identify and record them before analysis.

For a discovered directory, inspect the actual response and content. Do not assume a path is sensitive or useful merely because a wordlist found it.

## 3. Follow the evidence

The challenge may involve several stages. Keep a short record of each clue, the tool used, the result, and why it suggests the next step. If a clue points to a credential, confirm which service it belongs to before trying it. If a file contains encoded text, identify the likely format and decode one layer at a time.

Do not run unknown files on your normal computer. Use the lab's provided environment and keep all activity within the assigned machine.

## 4. Establish access carefully

If the evidence supports an authentication or application weakness, reproduce it with the smallest safe test. Verify access by checking the host and user identity. A response that looks successful is not enough if it does not demonstrate the expected access.

Useful post-login checks include whoami, id, hostname, pwd, and ls -la. Their output is specific to the target. Use it to understand the current account before looking for further access.

## 5. Investigate privilege escalation

Check sudo permissions, SUID files, scheduled tasks, writable scripts, and service configuration only as relevant to the target:

    sudo -l
    find / -perm -4000 -type f 2>/dev/null

These commands enumerate potential paths; they do not prove that every listed item is vulnerable. Verify the exact permissions, executable path, and relevant software behaviour before attempting an escalation.

## 6. Verify flags and document the path

Submit the user or root flag values from the live machine, not from a generic guide. In the public note, redact flags as flag{hidden}. Explain how the initial service led to access and how the privilege boundary was crossed, but do not claim a stage succeeded unless the target confirms it.

## What I took from this room

A themed CTF is still a methodical investigation. I enumerate the target, follow verified clues, establish the actual privilege level, and document the chain. This is more reliable than guessing based on the story or copying another machine's output.