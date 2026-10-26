# Operating Systems

Official room: https://tryhackme.com/room/operatingsystems:introduction

## Room Overview & Information

**Category:** Computer fundamentals  
**Difficulty:** Beginner  
**Topics:** Kernel and user space, processes, threads, memory, filesystems, permissions, services, logging, and updates.

An operating system (OS) manages hardware and offers controlled services to applications. It is both a resource manager and a set of security boundaries. Understanding what the OS does helps with everyday troubleshooting and with interpreting evidence during a security investigation.

This is a learning room, not a boot-to-root target. The practical work is to identify the operating-system concept described by each task and use the appropriate interface or command to inspect the active system.

## Phase 1: Inspect the Current System

On a Linux lab system, collect basic context before making changes:

```bash
whoami
id
uname -a
cat /etc/os-release
pwd
ps aux
df -h
free -h
```

- `whoami` prints the current username.
- `id` shows user and group identifiers.
- `uname -a` reports kernel and platform details.
- `cat /etc/os-release` displays distribution information.
- `pwd` prints the current working directory.
- `ps aux` lists processes.
- `df -h` shows filesystem capacity.
- `free -h` reports memory usage.

Use output from the room's own machine. Don't copy operating-system versions, account names, or memory values from a different VM.

On Windows, use Settings, Task Manager, System Information, and PowerShell to inspect the equivalent areas.

## Phase 2: Understand the Kernel and User Space

The **kernel** is the privileged core that schedules work, manages memory, and mediates access to devices and other resources. Applications normally execute in **user space** and request privileged operations through system calls or operating-system APIs.

This separation limits what a normal application can do directly. If an application is compromised, its permissions and accessible resources determine the impact. A kernel vulnerability can have a larger blast radius because the kernel controls critical system functions.

## Phase 3: Processes, Threads, and Services

A **program** is code stored on disk. A **process** is a running instance of that program with resources and an execution context. A **thread** is an execution path inside a process; multiple threads may share process memory and resources.

A **service** or **daemon** typically runs in the background to provide a function such as web hosting, logging, scheduling, or networking.

Linux inspection examples:

```bash
ps aux
top
pgrep -a -f PROCESS_NAME
systemctl --type=service --state=running
```

- `ps aux` provides a process snapshot.
- `top` shows an updating process view.
- `pgrep -a -f` searches command lines for a name or pattern.
- `systemctl` lists running systemd services.

A process name alone is not enough to decide whether it is malicious. Consider its executable path, parent process, owner, command line, start time, network connections, and expected role. Do not terminate an unfamiliar process before understanding its purpose.

On Windows, Task Manager lists processes and services, while PowerShell offers:

```powershell
Get-Process
Get-Service
```

## Phase 4: Memory, Storage, and Filesystems

RAM holds data used by active programs and is generally volatile. Persistent storage such as an SSD or HDD retains files across reboot. Virtual memory provides processes with address spaces while allowing the OS to manage physical RAM.

During incident response, memory can preserve process details, open connections, and data that may not be available from disk. Acquiring memory can change the live system, so the collection method should be chosen and documented carefully.

Linux commonly uses one directory tree rooted at `/`. Windows commonly uses drive letters and access-control lists. Both systems apply permissions to control who can read, modify, or execute resources.

Useful Linux commands:

```bash
ls -la PATH
stat PATH
file PATH
sha256sum PATH
```

These commands list file details, inspect metadata, identify the likely file type, and calculate a hash. A hash helps identify an exact file copy but does not prove that the file is trustworthy.

## Phase 5: Permissions and Privilege Boundaries

Linux permission bits determine read, write, and execute access for the owner, group, and others. Additional controls such as ACLs, capabilities, and mount options can further restrict access. Windows uses access-control lists, security tokens, and integrity levels.

**Least privilege** means giving each user or service only the rights required for its work. Running all applications as root or administrator increases the impact of a vulnerability.

Privilege escalation means gaining more permissions than intended. Examples include unsafe sudo rules, SUID binaries, writable privileged scripts, exposed service credentials, or a vulnerable kernel. Confirm a real permission path on the target rather than assuming that a group name alone proves an escalation method works.

## Phase 6: Startup, Services, Logs, and Updates

When a computer starts, firmware initialises hardware, a bootloader loads the OS kernel, and the system starts core services and the user environment. Secure Boot can help verify boot components where correctly configured, but it does not eliminate vulnerabilities in trusted software.

Services may run with specific user identities and start automatically. Inspect service configuration and permissions before changing them:

```bash
systemctl status SERVICE_NAME
systemctl cat SERVICE_NAME
journalctl -u SERVICE_NAME
```

Replace SERVICE_NAME with the actual service in the lab. The commands show service state, its unit configuration, and relevant journal entries.

OS logs can record logins, service failures, process events, and configuration changes. Logs can be incomplete or tampered with, so correlate more than one source and account for time-zone differences.

Security updates fix known vulnerabilities. Keep the OS and applications supported, restrict administrative access, and test that critical services still work after patching.

## Phase 7: Room Question and Answer Method

For each task, identify the component described by the question and read the live system or lesson content:

| Question focus | Relevant concept |
|---|---|
| Privileged core of the OS | Kernel |
| Running instance of a program | Process |
| Execution path inside a process | Thread |
| Temporary working memory | RAM |
| Persistent files and directories | Filesystem/storage |
| Background system component | Service or daemon |
| Limiting user/service rights | Least privilege |
| OS records of activity | Logs and auditing |
| Fix for known OS security flaws | Patching and updates |

These mappings are study guidance, not invented copies of the room's exact quiz wording. If a task asks for a machine-specific value, use the current room output.

## Phase 8: Key Takeaways & Remediation

Keep the OS supported and patched, restrict administrator accounts, remove unnecessary services, review privileged permissions, protect logs, and test backup restoration. During investigation, record user and process identity, preserve useful evidence, and distinguish observed facts from assumptions.

Processes, memory, filesystems, service identities, permissions, and logs all help explain what a system did and where a compromise may have occurred.
