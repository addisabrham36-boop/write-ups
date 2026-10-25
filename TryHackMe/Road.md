# Road: TryHackMe Walkthrough

Room: https://tryhackme.com/room/road

Road is a boot-to-root machine that asks for user and root flags. The exact application behaviour, target address, and successful path must be verified on the live machine. This walkthrough gives a repeatable workflow without inventing credentials or flag values.

## 1. Start the machine and enumerate services

Start the target, copy its current IP, and run:

    nmap -sC -sV -oN road-nmap.txt TARGET_IP

If the first scan does not reveal all relevant services, scan the full TCP range:

    nmap -p- --min-rate 1000 -oN road-allports.txt TARGET_IP

Use the current target address in place of TARGET_IP. Review the open ports and service versions, then focus on the services that are actually exposed.

## 2. Inspect the web application

Open the HTTP or HTTPS service in a browser. Record the application's purpose, visible links, login or registration pages, and any unusual behaviour. Check robots.txt and page source when relevant. Use a directory discovery tool only against the lab target and inspect results manually.

For a web application, map the roles and functions: what can an unauthenticated visitor do, what can a normal user change, and what is reserved for an administrator? This helps distinguish a missing authorisation check from a normal feature.

## 3. Investigate authentication and account functionality

If the room provides a login, registration, profile, or password-reset feature, test the expected workflow using lab accounts. Pay attention to whether the server validates the current user's identity before allowing a sensitive change. A client-side restriction is not enough if the same action can be performed directly through an HTTP request.

Use the room's evidence to establish the weakness. Do not assume a password-reset flaw, upload vulnerability, or administrator endpoint exists simply because it appears in another machine's write-up.

## 4. Examine file upload and server-side behaviour

If the application permits uploads, check the intended file types, size limits, server-side validation, and where uploaded files are stored. In an authorised lab, use only the proof required by the room. A filename extension check is weaker than validating content and storing uploads outside executable web paths.

If a server-side code-execution path is demonstrated, record the exact preconditions and evidence. Do not deploy a payload to a real service. Confirm the identity and permissions of any resulting shell before proceeding.

## 5. Enumerate the local system

After authorised access is obtained, inspect the current user and host:

    whoami
    id
    hostname
    pwd
    ls -la

Then examine sudo permissions, SUID binaries, scheduled jobs, writable scripts, and service configuration where appropriate:

    sudo -l
    find / -perm -4000 -type f 2>/dev/null

These commands are enumeration aids, not proof of a vulnerability. Verify the permissions and intended behaviour of any candidate before using it.

## 6. Verify the flags and write up the chain

The room asks for user and root flags. Locate them only on the active target and submit the values there; the public note does not reproduce them. Verify root access with whoami and id before claiming the final stage succeeded.

A complete report should explain the initial service exposure, the web weakness, the path to user access, the privilege escalation mechanism, and the remediation for each issue. Depending on the actual finding, fixes may include stronger server-side authorisation, safer upload handling, credential rotation, least privilege, and correcting unsafe service permissions.

## Troubleshooting

- No ports appear open: confirm the target is running and the IP is current.
- The web app is unavailable: verify the correct port and allow the machine time to boot.
- A login or reset workflow differs from a guide: follow the active room's UI and record the actual behaviour.
- A shell has limited permissions: enumerate the current context before attempting escalation.
- A candidate escalation does not work: check the exact binary path, permissions, and target version rather than repeating commands blindly.

## What I took from this room

Road should be solved as a chain of verified observations. Each stage must follow from the previous one, and a final flag is meaningful only after the access path and privilege level have been confirmed on the live machine.