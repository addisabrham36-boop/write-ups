# Operating Systems: TryHackMe Walkthrough

Room: https://tryhackme.com/room/operatingsystems:introduction

An operating system manages the hardware and provides services to applications. It handles process execution, memory, files, devices, user accounts, permissions, and networking. Understanding these functions is useful for troubleshooting and for investigating security incidents.

## 1. Kernel and user space

The kernel runs with high privilege and manages core resources. Ordinary applications run in user space and request operating-system services through APIs or system calls. This separation limits what a normal application can do directly.

A kernel vulnerability can have a broader impact than a flaw in a single unprivileged application because the kernel controls access to many system resources.

## 2. Processes and threads

A process is a running program with its own execution context and resources. A thread is an execution path within a process. Several threads can share process resources while doing different work.

On Linux, ps aux lists processes, top provides a live process view, and pgrep can search by name or pattern. On Windows, Task Manager and PowerShell commands such as Get-Process provide similar information. A process name alone is not enough to decide whether it is malicious; inspect its path, parent, identity, behaviour, and network activity.

## 3. Memory and storage

RAM holds data used by active programs and is generally volatile. Persistent storage retains files after shutdown. The operating system manages virtual memory so processes can use an address space without directly controlling physical RAM.

During incident response, memory may preserve running-process details or network connections that are absent from a disk image. Acquiring memory changes the system to some degree, so the method should be selected and documented carefully.

## 4. Filesystems and permissions

Linux commonly uses one directory tree rooted at /. Windows commonly uses drive letters and access-control lists. Both systems enforce permissions to control who can read, write, execute, or administer resources.

Least privilege means users and services receive only the rights needed for their work. Running every application as root or administrator increases the impact of bugs and compromise.

Useful Linux inspection commands include ls -la, id, whoami, df -h, and free -h. They show directory details, user identity and groups, filesystem capacity, and memory information.

## 5. Services and startup

Services run in the background to provide functions such as networking, logging, web hosting, or scheduled jobs. A service may start automatically and may run with a particular user identity. An unsafe service configuration or writable startup script can create a privilege-escalation path.

Inspect service configuration and permissions before changing anything. Do not terminate unfamiliar system services without understanding their role.

## 6. Logs and updates

Operating systems record events such as logins, service failures, process activity, and configuration changes, depending on the logging settings. Logs help reconstruct a timeline, but they can be incomplete or tampered with. Correlate multiple sources and check timestamps.

Updates fix known vulnerabilities and improve stability. Keep the OS and applications supported, restrict administrative access, and verify that critical services continue to work after patching.

## 7. Working through the room

For each task, identify whether it describes the kernel, process management, memory, storage, permissions, services, or logging. If the task asks you to inspect the machine, use the command or interface it provides and read the live output. Avoid assuming every operating system has identical paths or tools.

## What I took from this room

The operating system is a resource manager and a security boundary. Processes, memory, filesystems, service identities, permissions, and logs all help explain how a system behaves and where a compromise may have occurred.

## Phase 1: Inspect the current system

Record the operating system, current account, and basic resource state before making changes:

```bash
whoami
id
uname -a
cat /etc/os-release
ps aux
df -h
free -h
```

These commands identify the user and groups, kernel/platform details, OS release, running processes, filesystem capacity, and memory usage. The exact output depends on the machine; use the active lab rather than a copied transcript.

On Windows, similar information can be obtained through Settings, Task Manager, System Information, and PowerShell.

## Phase 2: Understand privilege boundaries

The kernel mediates access to hardware and sensitive system resources. Applications normally run in user space and request privileged operations through system calls or operating-system APIs. Least privilege reduces the damage a compromised process can cause.

Linux file permissions describe read, write, and execute access for the owner, group, and others; Windows uses access-control lists and security tokens. A process name or account name alone does not establish its effective permissions. Verify the identity, groups, execution context, and access rules.

## Phase 3: Processes, services, and startup

A process is an executing program, while a thread is an execution path within a process. Services run background work such as networking, logging, web hosting, and scheduled jobs. A service that runs with excessive privileges or executes a writable script can create an escalation path.

Inspect unknown processes before terminating them. Record their path, parent process, owner, command line, and network behaviour. During incident response, avoid actions that destroy volatile evidence unless containment requires them.

## Phase 4: Filesystems, logs, and incident investigation

Persistent files may survive reboot, while RAM and some process state are volatile. Forensic collection may need to preserve both, depending on the incident. File timestamps can be modified, and local logs may be incomplete or tampered with, so correlate multiple sources.

For a Linux file of interest, useful commands include:

```bash
ls -la PATH
stat PATH
file PATH
sha256sum PATH
```

These show directory metadata, detailed file metadata, likely file type, and a byte-level fingerprint. A hash can demonstrate that two copies match; it does not independently establish authenticity.

## Phase 5: Room tasks and remediation

Map each task to the relevant concept: kernel/user space, processes, RAM/storage, filesystem, permissions, services, or logs. Use commands in the room environment and record the exact result.

Keep the OS supported, patch known vulnerabilities, restrict administrative accounts, remove unnecessary services, review privileged permissions, protect logs, and test backup recovery. Do not run the entire system as root simply to avoid fixing permission problems.
