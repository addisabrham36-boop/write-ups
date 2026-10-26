# Introduction to Operating Systems

Official room: https://tryhackme.com/room/introtooperatingsystems

## Room Overview & Information

**Category:** Computer fundamentals  
**Difficulty:** Beginner  
**Topics:** Operating-system responsibilities, kernel and user space, processes, memory, filesystems, permissions, services, boot, and updates.

This is a conceptual learning room rather than a boot-to-root target. There is no exploitation chain to fabricate. The walkthrough below maps the key concepts to practical inspection commands and explains how to answer the room's knowledge checks using the correct terms.

## Phase 1: Understand the Operating-System Role

An operating system (OS) manages the computer's hardware and provides a controlled environment for applications. It mediates access to CPU time, memory, storage, devices, network interfaces, user identities, and permissions. Without an operating system, each application would need to implement many low-level functions and safely share the hardware with other programs.

Examples include Windows, Linux distributions, macOS, Android, and iOS. Android uses the Linux kernel but has its own application framework and security model; the name of the kernel alone does not describe the entire operating system.

### Key responsibilities

| Responsibility | What the OS does | Security relevance |
|---|---|---|
| Process management | Starts, schedules, and stops processes | Helps identify unexpected execution |
| Memory management | Allocates memory and separates process address spaces | Isolation failures can expose data or allow compromise |
| Filesystem management | Stores files and enforces access rules | Incorrect permissions can expose sensitive data |
| Device management | Provides drivers and controlled access to hardware | Drivers and device interfaces can expand privilege |
| User and permission management | Verifies identities and controls resource access | Least privilege limits the impact of compromise |
| Networking | Provides sockets and protocol support | Listening services and connections appear in investigations |
| Logging and auditing | Records selected events | Helps reconstruct incidents and verify actions |

## Phase 2: Inspect the Kernel, Processes, and Services

### 1. Kernel and user space

The kernel is the privileged core of an operating system. It schedules processes, manages memory, and controls access to hardware and system resources. Ordinary applications usually run in user space and request privileged operations through system calls or operating-system APIs.

This separation is a security boundary. A normal application should not be able to freely overwrite another process's memory or change kernel settings. A kernel vulnerability can have a broader impact because the kernel mediates access for many programs.

### 2. Processes and services

A program is a set of instructions stored on disk; a process is a running instance of that program. A thread is an execution path within a process. A service or daemon commonly runs in the background to provide a continuing function such as web hosting, networking, or logging.

On Linux, inspect process and service information:

```bash
ps aux
systemctl --type=service --state=running
```

- `ps aux` lists processes with user, resource, and command information.
- `systemctl --type=service --state=running` lists systemd services currently running.

A process name alone does not establish whether activity is malicious. Check its path, parent process, account, command line, network connections, and expected role. Do not terminate an unfamiliar production process before understanding its purpose.

On Windows, Task Manager and Services provide graphical views. PowerShell offers corresponding commands:

```powershell
Get-Process
Get-Service
```

### 3. Memory and storage

RAM stores information used by active processes and is generally volatile: the contents are lost when power is removed. Persistent storage, such as an SSD or HDD, holds files across reboots. Virtual memory provides processes with an address-space abstraction and lets the OS manage physical memory.

Memory can preserve evidence of processes, connections, and decrypted content that may not remain on disk. Capturing memory can also change the live system, so forensic collection should be planned and documented.

## Phase 3: Filesystems, Users, and Permission Boundaries

Linux commonly represents files beneath one root directory, `/`. Windows commonly uses drive letters such as `C:\` and access-control lists (ACLs). Both operating systems apply access rules to decide which users or processes can read, write, or execute resources.

Useful Linux commands:

```bash
whoami
id
pwd
ls -la
stat PATH
df -h
free -h
```

- `whoami` prints the current effective username.
- `id` displays user and group IDs.
- `pwd` shows the current working directory.
- `ls -la` lists normal and hidden entries, including permission details.
- `stat` displays metadata for a particular file or directory.
- `df -h` shows filesystem capacity.
- `free -h` reports memory usage.

Replace PATH with the file or directory being examined. These commands are informational; they do not independently prove a system is secure or compromised.

### Least privilege and privilege escalation

Least privilege means each user and service receives only the rights needed for its job. Running an entire web application as root or giving routine users unrestricted administrator access increases the impact of a bug or malicious file.

Privilege escalation occurs when a user or process gains more authority than intended. Common causes include overly broad sudo rules, unsafe SUID binaries, writable privileged scripts, exposed service credentials, or vulnerable kernel components. The correct finding must be based on actual permissions and configuration, not a group's name alone.

## Phase 4: Boot, Services, Updates, and Troubleshooting

### 1. Boot process

When a computer starts, firmware initialises hardware and selects a boot device. A bootloader then loads the operating-system kernel and related startup data. The kernel initialises core services and the system transitions into the normal user environment.

Secure Boot can help prevent unauthorised boot components from loading when it is correctly configured. It does not eliminate vulnerabilities in trusted software or protect a system after a user-authorised process has been compromised.

### 2. Services and startup configuration

Services run background tasks, often under a dedicated account. A service with excessive permissions or a writable startup script can become a privilege-escalation path. Review configuration and permissions before editing or restarting a service.

On systemd-based Linux, service status can be inspected with:

```bash
systemctl status SERVICE_NAME
systemctl cat SERVICE_NAME
journalctl -u SERVICE_NAME
```

Replace SERVICE_NAME with the actual service name. The first command shows status, the second displays the unit definition, and the third reads its journal records.

### 3. Updates and troubleshooting

Security updates patch known vulnerabilities and improve reliability. Organisations should keep supported operating systems and applications current, test critical updates where practical, and verify that services recover correctly.

When something fails, identify the layer involved:
1. Read the exact error message.
2. Check whether the process or service is running.
3. Verify the user's identity and permissions.
4. Check disk space and memory pressure.
5. Inspect relevant logs.
6. For network services, check the listening address, port, route, and firewall.
7. Record the result before changing another variable.

Do not assume every crash is malware or every permission error means the filesystem is broken.

## Phase 5: Room Questions & Answer Method

For each knowledge check, identify which OS responsibility the question describes. Match the wording to the concept rather than guessing from an example.

| Question focus | Concept to identify |
|---|---|
| Software that manages hardware and applications | Operating system |
| Privileged core that manages resources | Kernel |
| Running instance of a program | Process |
| Execution path inside a process | Thread |
| Temporary, volatile working memory | RAM |
| Persistent long-term data | Storage / filesystem |
| Background component providing a function | Service or daemon |
| Limiting permissions to required tasks | Least privilege |
| Operating-system component that starts after firmware/bootloader | Kernel and startup services |
| Fixes for known OS vulnerabilities | Security updates/patching |

These are concept mappings, not invented copies of the live quiz's exact wording. If the task asks for a specific string or interface value, use the active room's prompt and answer choices.

## Phase 6: Key Takeaways & Remediation

An operating system is both a resource manager and a set of security boundaries. Harden it by keeping software supported and patched, restricting administrator access, removing unnecessary services, securing startup configuration, reviewing privileged permissions, collecting protected logs, and testing backups.

For investigation, verify user and process identity, preserve useful evidence, and separate observations from assumptions. The OS provides the context needed to understand what a process can access and how an unexpected action could have occurred.
