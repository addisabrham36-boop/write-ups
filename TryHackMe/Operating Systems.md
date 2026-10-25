# Operating Systems — TryHackMe

Room: https://tryhackme.com/room/operatingsystems

An operating system manages the computer's hardware and provides services that applications use. It handles processes, memory, files, devices, users, and permissions. For cybersecurity, understanding the operating system helps me interpret what a process is doing and why access is allowed or denied.

## Processes and memory

A process is a running program with its own execution state and resources. The operating system schedules processes so they can share the CPU. Memory management keeps track of which parts of memory belong to which processes and helps isolate them from one another.

On Linux, I can inspect running processes with:

```bash
ps aux
top
```

The exact output differs between systems. A process name alone does not prove that a program is safe or malicious; I would need to investigate its path, arguments, owner, network activity, and other context.

## Files and permissions

The file system organises files and directories. On Unix-like systems, permissions control which users can read, write, or execute a file. The owner, group, and other-user permission bits are important when investigating access problems.

```bash
ls -l
id
```

The first command displays file details and permissions; the second shows the current user's identity and group memberships.

## Users and privilege

Normal users should not automatically have unrestricted control over the system. Administrative privileges are powerful and should be limited. Misconfigured permissions, exposed credentials, or unnecessarily privileged services can create security weaknesses.

## What I took from this room

The operating system is the layer that manages processes, memory, files, devices, and users. Learning its basic behaviour gives me a foundation for both defensive investigations and authorised penetration testing. Commands provide clues, but I need to interpret them in context rather than assume a suspicious-looking process is automatically malicious.