# Internal: TryHackMe Walkthrough

Room: https://tryhackme.com/room/internal

Internal is a black-box penetration-testing challenge. The brief authorises an external, web application, and internal assessment of the assigned virtual machine, with user and root flags as proof of impact. The scope is the IP assigned by the room, and the hostname internal.thm must resolve to that target.

## 1. Confirm scope and hostname

Start the target and record the assigned IP. Add the following mapping to /etc/hosts on the AttackBox or testing machine:

    MACHINE_IP internal.thm

Replace MACHINE_IP with the current target address. Verify resolution before using the hostname. Only the assigned target is in scope; do not scan unrelated systems.

## 2. Enumerate ports and services

Start with a version and default-script scan:

    nmap -sC -sV -oN internal-nmap.txt MACHINE_IP

If needed, scan all TCP ports:

    nmap -p- --min-rate 1000 -oN internal-allports.txt MACHINE_IP

Run a focused scan against discovered ports after the broad scan. Save the results and identify web services, SSH, and any other exposed services. A version banner is a lead; verify whether the relevant vulnerability or misconfiguration applies.

## 3. Inspect the web application

Browse to the target hostname. Inspect the homepage, robots.txt, source comments, directories, login pages, and any exposed content management system. Use the room's clues to identify relevant paths instead of blindly trusting an old wordlist result.

If a WordPress instance is present, note its public version clues, user-facing pages, login endpoint, and any configuration exposure. Do not assume the administrator username or password. Enumerate usernames and authentication behaviour only within the authorised target and at a controlled rate.

## 4. Validate the initial access path

The room may require a chain involving a web application weakness or credentials. Establish the path from evidence: identify the affected endpoint, determine what input or authentication weakness exists, and verify the resulting access. If a reverse shell is part of the task, use only the lab-provided environment and ensure the callback address and port are reachable from the target.

Record the exact request, response, and evidence of the resulting access. A successful-looking web response is not enough; verify the shell identity and target host.

## 5. Enumerate the user context

After obtaining a shell, collect basic information:

    whoami
    id
    hostname
    pwd
    ls -la

Inspect application configuration and environment files only where permitted and relevant to the room. Credentials found in configuration should be treated as sensitive and used only to validate the intended lab path. Never copy real secrets into a public write-up.

Search for privilege escalation opportunities by reviewing sudo permissions, SUID binaries, scheduled tasks, writable scripts, and service configuration:

    sudo -l
    find / -perm -4000 -type f 2>/dev/null

The results are specific to the live target. Verify a candidate weakness before using it.

## 6. Escalate and verify

If the lab's intended path allows root access, perform the minimum required action and verify the identity with whoami and id. Then locate the root flag according to the room instructions. The flag itself is not included here; submit the value from your own instance.

Document how the weakness enabled escalation and what control would prevent it. Common fixes may include patching the application, removing exposed configuration, rotating credentials, limiting sudo rights, and correcting unsafe file permissions.

## 7. Write the assessment findings

For each vulnerability, record the affected component, evidence, preconditions, impact, and remediation. Keep the chain clear: initial exposure, user access, local enumeration, privilege escalation, and verified impact. Avoid claiming a step worked unless the target output supports it.

## What I took from this room

Internal is best approached as a real engagement: confirm scope, enumerate systematically, follow evidence, verify each stage of access, and document the weaknesses so the owner can fix them. Metasploit is optional; understanding the target and the chain is more important than a particular tool.