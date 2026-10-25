# Introduction to Operating Systems: TryHackMe Walkthrough

Room: https://tryhackme.com/room/introtooperatingsystems

An operating system sits between applications and the hardware. It manages processes, memory, files, devices, users, and permissions so applications can run without each program needing to manage the hardware itself.

## 1. Main operating-system responsibilities

- **Process management:** creates, schedules, and stops running programs.
- **Memory management:** allocates memory and isolates processes.
- **File-system management:** organises files and directories and controls access.
- **Device management:** provides drivers and interfaces for hardware.
- **User and permission management:** controls identities and resource access.
- **Networking:** provides APIs and services that allow programs to communicate.
- **Logging and auditing:** records selected events for troubleshooting and security.

The kernel is the privileged core of the OS. User applications normally run with fewer privileges. This separation helps contain failures and restricts what a compromised program can do.

## 2. Processes and services

A program is a set of instructions stored on disk; a process is a running instance of that program. A service or daemon commonly runs in the background to provide a continuing function, such as networking, logging, or web hosting.

On Linux, inspect processes and services with the commands ps aux and systemctl --type=service --state=running. The first lists processes, and the second lists running systemd services. Use these commands to understand the system, not to terminate unfamiliar processes blindly.

On Windows, Task Manager and Services provide graphical views. PowerShell commands such as Get-Process and Get-Service can also inspect processes and services.

## 3. Filesystems and permissions

Filesystems organise persistent data. Linux commonly uses a single directory tree rooted at /, while Windows commonly uses drive letters such as C:\. The visible layout differs, but both systems have files, directories, ownership or ACLs, and access rules.

Linux permissions commonly distinguish owner, group, and other users, with read, write, and execute bits. Windows commonly uses access-control lists to grant or deny specific rights. The principle is the same: users and processes should receive only the access they require.

Useful Linux checks are pwd, ls -la, id, and whoami. These show the current directory, directory contents and permissions, group memberships and identity details, and current username.

## 4. Users, privilege, and isolation

An ordinary user should not have unrestricted control over the operating system. Administrative roles are needed for system changes, but using them for every task increases the impact of mistakes or compromise.

Privilege escalation occurs when a user or process gains permissions beyond those intended. Defenders reduce the risk through timely patching, minimal administrative access, secure service configuration, and monitoring for unexpected permission changes.

## 5. Boot and updates

The OS loads after firmware and boot components initialise the system. Once running, services start according to the system's configuration. Security updates fix known vulnerabilities, so update management is a key part of keeping the OS secure.

A production update process should test critical updates where practical, maintain backups, and verify that systems restart and services remain healthy after patching.

## 6. Troubleshooting by layer

If an application fails, check the error message, process state, permissions, available disk space, memory pressure, and relevant logs. For a service, verify that it is running and listening on the expected address and port. For a file-access error, inspect ownership and permissions before changing them.

Do not fix every problem by running the application as administrator or root. That can hide the underlying issue while increasing the consequences of a future vulnerability.

## 7. Working through the room

For each task, identify the operating-system function described. Process scheduling belongs to process management, persistent file organisation belongs to filesystem management, and access restrictions belong to permissions and security. Use the task's terminology and inspect the provided environment when it asks for a command or value.

## What I took from this room

The operating system is both a resource manager and a security boundary. Processes, permissions, filesystems, services, and updates all affect how safely a computer operates.

## Expanded operating-system study guide

### 1. Purpose and kernel

An operating system provides a consistent interface between applications and hardware. The kernel manages privileged operations, process scheduling, memory, devices, filesystems, and system calls. User-space programs request services from the kernel rather than directly controlling hardware.

### 2. Process and memory management

A process is an executing program with its own resources and address space. Threads are execution paths within a process. Virtual memory gives processes a controlled address space and helps isolate them from one another. RAM is volatile; disk or flash storage is persistent.

### 3. Filesystems and permissions

Filesystems organize data into directories and metadata. Linux commonly uses a single root directory, while Windows commonly exposes drive letters. Permissions and access-control lists determine which users and processes can access resources.

On Linux, commands such as `whoami`, `id`, `ls -la`, `df -h`, and `free -h` can inspect identity, groups, file permissions, storage capacity, and memory. Run commands in the lab and interpret the actual output.

### 4. Services and startup

Services perform background functions such as networking, logging, and web hosting. Startup configuration determines which services run automatically. Unnecessary services increase attack surface, and services running with excessive privileges can amplify the impact of a compromise.

### 5. Users and privilege

Normal users should have only the permissions required for their tasks. Administrative accounts should be protected and used only when needed. On Linux, root is the superuser; on Windows, SYSTEM is a highly privileged service identity. They are not interchangeable names for the same account.

### 6. Logs, patching, and security

Logs record events such as authentication attempts, service failures, and system changes. Their usefulness depends on coverage, timestamps, retention, and integrity. Updates address known vulnerabilities, while secure configuration reduces unnecessary exposure.

### 7. Answering room questions

Determine whether the question refers to the kernel, shell, process, filesystem, memory, user permissions, or a service. Use the terminology from the active room. A shell is an interface for issuing commands; it is not the operating system itself.

### 8. Practical takeaway

The operating system mediates access to resources and enforces privilege boundaries. Understanding its responsibilities is essential for troubleshooting, hardening, and incident response.
