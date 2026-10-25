# Windows Basics: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/windowsbasics

## Room Overview & Information

**Category:** Operating-system fundamentals  
**Difficulty:** Beginner  
**Focus:** Windows interface, filesystem, users, processes, services, and basic administrative tools.

Windows is widely used on personal devices and enterprise networks. Security analysts need to understand how users log in, where files live, how programs run, how services start, and where system events are recorded.

## Phase 1: Navigate the interface and filesystem

Windows commonly uses drive letters such as `C:\`. The system directory is often `C:\Windows`, and user profiles are commonly stored under `C:\Users\USERNAME`. Paths and installed applications vary by machine.

In Command Prompt, use:

```cmd
whoami
cd
dir
dir /a
type notes.txt
```

- `whoami` prints the current identity.
- `cd` shows or changes the current directory.
- `dir` lists directory entries.
- `dir /a` includes hidden and system entries.
- `type` prints a text file.

In PowerShell, similar commands include `Get-Location`, `Get-ChildItem -Force`, and `Get-Content notes.txt`.

## Phase 2: Users and groups

A user account represents an identity. Groups help assign permissions to multiple users. Membership in the local Administrators group gives elevated rights and should be limited to accounts that need them.

Inspect identity and group membership:

```cmd
whoami /all
net user
net localgroup
```

`whoami /all` shows the current identity, groups, and privileges. `net user` lists local user accounts, and `net localgroup` lists local groups. Some information may require administrative permissions.

Do not assume a user is an administrator because their username looks important. Check the group membership and effective privileges.

## Phase 3: Processes and services

A process is a running program. A service is a background component that can start automatically and provide functions such as networking, logging, or application hosting.

In PowerShell:

```powershell
Get-Process
Get-Service
Get-EventLog -LogName System -Newest 20
```

The first command lists processes, the second lists services, and the third shows recent events from the legacy System event log interface where supported. Modern systems may use other PowerShell event-log cmdlets and providers.

In Task Manager, inspect the process name, resource usage, and details. A suspicious name alone is not enough to prove malware. Check the executable path, publisher, parent process, user, and network activity where available.

## Phase 4: Windows Event Viewer

Event Viewer organises logs into categories such as Application, Security, and System. Security events can show authentication and account activity when the relevant audit policy is enabled. System events can show service and driver issues. Application logs may show errors from software.

When investigating an event, record the timestamp, event ID, source, account, computer, and message. Event IDs are useful, but the same number can mean different things across providers. Read the event source and details rather than relying on the ID alone.

## Phase 5: Basic network inspection

```cmd
ipconfig /all
route print
netstat -ano
nslookup example.com
```

- `ipconfig /all` shows interface addresses, DNS configuration, and other network details.
- `route print` displays the routing table.
- `netstat -ano` lists connections/listeners, numeric addresses, and associated process IDs.
- `nslookup` queries DNS.

These commands help distinguish a name-resolution issue from a routing or service problem. Do not expose full network configuration publicly if it includes sensitive internal information.

## Phase 6: Working through the room

For each question, use the interface or command specified by the task and read the value from the lab. If the task asks for a hidden file, inspect hidden entries; if it asks for a user or process, verify it using the appropriate tool. Do not copy account names, file paths, or output from a different Windows instance.

## Phase 7: Security lessons

- Use standard user accounts for daily work and elevate only when required.
- Keep Windows and applications patched.
- Enable useful audit policies and forward logs centrally.
- Restrict remote administration and exposed services.
- Review service permissions and startup entries.
- Use endpoint protection and investigate suspicious process behaviour.
- Protect backups and credentials.

## Key takeaways

Windows administration and security investigation rely on understanding identity, permissions, processes, services, logs, and networking. The best first step is to inspect the actual system state and use evidence, rather than assuming that every Windows machine has the same configuration.