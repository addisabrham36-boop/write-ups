# Windows Basics — TryHackMe

Room: https://tryhackme.com/room/windowsfundamentals1xbxzy

Windows provides a graphical desktop as well as command-line tools and administrative interfaces. For security work, I need to understand how users, services, processes, files, and logs fit together.

## Useful built-in tools

Task Manager shows running applications and processes. Services management shows background services and their status. Event Viewer stores logs from Windows components and applications. These tools help me understand what a machine is doing, but I should not assume that every unfamiliar process is malicious.

The Command Prompt and PowerShell can also provide useful information. For example:

```powershell
whoami
ipconfig
tasklist
```

`whoami` shows the current identity, `ipconfig` displays network configuration, and `tasklist` lists running processes. I should run commands in the environment provided by the room and compare the output with the task's instructions.

## Files, users, and updates

Windows uses access controls to decide which users can read or change files and perform administrative actions. Keeping the operating system and applications updated helps reduce exposure to known vulnerabilities. Services that are not needed should be reviewed, but disabling system components without understanding their role can break things.

## Event logs

Windows records many system and security events. The Security log can contain events related to logons and account activity, depending on audit policy. System and Application logs provide other useful context. Log availability depends on configuration and retention, so the absence of an event is not always proof that something did not happen.

## What I took from this room

I need to be comfortable navigating Windows, checking basic network information, reviewing processes and services, and finding logs. These are everyday skills for both system administration and security investigations. The important part is knowing what each tool can show and what its output cannot prove by itself.