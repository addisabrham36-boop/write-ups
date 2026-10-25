# Introduction to Operating Systems: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/introtooperatingsystems

## Room Overview & Information

**Category:** Computer fundamentals  
**Difficulty:** Beginner  
**Topics:** Kernel, processes, memory, filesystems, permissions, services, boot, and updates.

An operating system sits between applications and the hardware. It manages processes, memory, files, devices, users, and permissions so applications can run without each program having to manage hardware directly. This room is a learning exercise rather than a boot-to-root target.

## Phase 1: Reconnaissance & Operating-System Inventory

### 1. Identify the system

On a Linux lab machine, basic information can be inspected with:

```bash
whoami
id
uname -a
cat /etc/os-release
```

These commands show the current username, group and identity information, kernel/platform details, and operating-system release. Use the actual environment's output; do not copy a version number from another machine.

On Windows, similar information is available in Settings, System Information, Task Manager, and PowerShell.

## Phase 2: Operating-System Components and Access Boundaries

### 2. Main operating-system responsibilities

- **Process management:** creates, schedules, and stops running programs.
- **Memory management:** allocates memory and isolates processes.
- **Filesystem management:** organises data and controls file access.
- **Device management:** drivers and interfaces let the OS communicate with hardware.
- **User and permission management:** controls identities and resource access.
- **Networking:** provides APIs and services that let programs communicate.
- **Logging and auditing:** records selected events for troubleshooting and security.

The kernel is the privileged core of the OS. User applications normally run with fewer privileges and ask the kernel to perform protected operations through system calls.

### 3. Processes and services

A program is a set of instructions stored on disk; a process is a running instance of that program. A service or daemon often runs in the background to provide a continuing function such as networking, logging, or web hosting.

Inspect Linux processes and running systemd services:

```bash
ps aux
systemctl --type=service --state=running
```

The first command lists processes. The second lists currently running systemd services. Understand the process owner and purpose before terminating an unfamiliar process. Windows equivalents include Task Manager, Services, Get-Process, and Get-Service.

### 4. Filesystems and permissions

Linux commonly uses a single directory tree rooted at slash, while Windows commonly uses drive letters such as C:. Both systems have files, directories, owners or ACLs, and access rules.

Useful Linux checks:

```bash
pwd
ls -la
id
whoami
```

These show the current directory, entries and permission bits, group memberships, and username. Linux permissions commonly distinguish owner, group, and others with read, write, and execute bits; Windows uses access-control lists and security tokens.

### 5. Users, privilege, and isolation

An ordinary user should not have unrestricted control over the operating system. Administrative permissions are needed for some system changes, but using them for every task increases the impact of mistakes or compromise.

Privilege escalation occurs when a user or process gains permissions beyond those intended. Defenders reduce risk through patching, minimal administrative access, secure service configuration, and monitoring for unexpected permission changes.

### 6. Boot and updates

The OS loads after firmware and boot components initialise the system. Services start according to configuration. Security updates fix known vulnerabilities, so patching is a key part of operating-system security.

## Phase 3: Practical Investigation & Troubleshooting

If an application fails, check the error message, process state, permissions, available disk space, memory pressure, and relevant logs. For a service, verify that it is running and listening on the expected address and port. For a file-access error, inspect ownership and permissions before changing them.

Linux examples:

```bash
df -h
free -h
ss -lntup
journalctl -p warning -b
```

- **df -h** displays filesystem capacity in readable units.
- **free -h** displays memory usage.
- **ss -lntup** lists listening TCP/UDP sockets.
- **journalctl -p warning -b** shows warning-and-higher journal messages from the current boot, subject to permissions and logging configuration.

Do not solve every issue by running the application as administrator or root. That can hide the underlying problem and increase the consequences of a vulnerability.

## Phase 4: Room Questions & Evidence Walkthrough

For each task, identify the operating-system function being described. Process scheduling belongs to process management; persistent file organisation belongs to filesystem management; and access restrictions belong to permissions and security. Use the task's exact terminology and inspect the provided environment when it asks for a command or value.

This room's quiz wording can change. Use the active task's question and answer options for exact submission values rather than guessing an unprovided flag or terminal output.

## Phase 5: Key Takeaways & Remediation

The operating system is both a resource manager and a security boundary. Keep it supported and patched, restrict administrative accounts, remove unnecessary services, protect logs, review privileged permissions, and verify backup restoration. Processes, permissions, filesystems, services, and updates all affect the security and reliability of the computer.