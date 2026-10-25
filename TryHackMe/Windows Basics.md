# Windows Basics: TryHackMe Walkthrough

Room: https://tryhackme.com/room/windowsbasics

Windows is widely used on personal computers and in enterprise environments. For security work, it is useful to understand the filesystem, user accounts, processes, services, networking, and event logs. These are the places an analyst often checks when investigating a host.

## 1. Windows filesystem

Windows commonly uses drive letters such as C: and directories such as C:\Windows, C:\Users, and C:\Program Files. User profile directories store settings and user data. Application binaries and configuration may be placed in different locations depending on the installer and system policy.

In File Explorer, enable the view options required to inspect hidden files when investigating a system. Hidden files are not encrypted or protected merely because they are hidden. Access is controlled by permissions.

## 2. Users and permissions

Windows accounts may be local or managed through a domain. Group membership affects what actions a user can perform. Administrator privileges should be limited because a compromised administrator account can affect the whole machine.

In PowerShell, useful identity commands include whoami, whoami /groups, and Get-LocalUser. Use them to inspect the current account and local accounts in an authorised environment. The output depends on the machine and permissions.

## 3. Processes and services

A process is a running program. A service is a background component that can start automatically or on demand. Unexpected processes, unfamiliar executable paths, and services configured to run with excessive privileges can be useful investigation leads.

Useful PowerShell commands include Get-Process and Get-Service. Task Manager provides a graphical view of processes and startup activity. Do not terminate an unfamiliar process until you understand what it does; it may be a critical system component or evidence in an incident.

## 4. Command Prompt and PowerShell

Command Prompt and PowerShell are command-line environments, but they have different syntax and capabilities. PowerShell works with structured objects and provides cmdlets for system administration.

Examples of basic commands:
- whoami: show the current identity.
- ipconfig /all: display network configuration.
- netstat -ano: show network connections and associated process IDs.
- Get-Process: list running processes in PowerShell.
- Get-Service: list services in PowerShell.
- Get-EventLog or Get-WinEvent: query event logs, subject to permissions and available logs.

Run commands in the appropriate shell. A Command Prompt command may not work unchanged in PowerShell, and a PowerShell cmdlet is not a standard executable in every shell.

## 5. Networking

ipconfig /all shows interface addresses, gateways, DNS servers, and related configuration. Test a connection with an approved diagnostic tool and separate name resolution from network reachability. A failed ping does not prove a host is offline because ICMP may be blocked.

netstat -ano can show listening ports and active connections, including the process ID. Correlate that ID with a process listing before drawing conclusions.

## 6. Event logs

Windows Event Viewer organises records into logs such as System, Application, and Security. Security logs can include logon events, account changes, and privilege-related activity when auditing is configured. PowerShell can query logs, but access depends on privileges and policy.

During an investigation, record the event ID, timestamp, account, host, and message. One event rarely proves an entire attack chain. Correlate authentication events with process, network, and endpoint records.

## 7. Working through the room

Follow the task's exact environment and identify whether it asks about a path, command, account, service, process, or log. Use the tool shown in the task and read the actual output. Do not assume your personal Windows machine has the same users or files as the room's machine.

## What I took from this room

Windows security work starts with knowing where to find accounts, processes, services, network configuration, and event logs. Understanding these basics makes it easier to investigate alerts and recognise suspicious changes.