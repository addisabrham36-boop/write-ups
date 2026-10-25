# Introduction to Operating Systems — TryHackMe

An operating system manages hardware resources and provides services to programs. It handles processes, memory, storage, devices, users, and permissions, making it possible for applications to run without each one needing to control hardware directly.

## Processes and files

A process is a running instance of a program. The operating system schedules CPU time and manages memory so processes can share the machine. Files and directories organise persistent data, while access controls determine which users can read or change it.

On Linux, I can inspect the current identity and processes with:

```bash
whoami
ps aux
ls -l
```

These commands show the current user, running processes, and file permissions. They are starting points for investigation, not proof that a process or file is safe.

## Why it matters for security

Misconfigured permissions, outdated software, and overly privileged services can create weaknesses. Logs and process information also help defenders investigate suspicious behaviour. I should understand the normal role of a process before deciding it is malicious.

## What I took from this topic

The operating system is the layer that coordinates applications and hardware. Learning how it manages processes, files, users, and permissions makes later security tasks much easier to understand.