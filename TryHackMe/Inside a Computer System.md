# Inside a Computer System: TryHackMe Walkthrough

Room: https://tryhackme.com/room/insideacomputersystem

This room introduces the main parts of a computer and explains how hardware and software cooperate to execute instructions. Security problems can occur at several layers, from a vulnerable application down to firmware or physical access.

## 1. Core hardware components

- **CPU:** executes instructions and coordinates processing.
- **RAM:** temporary working memory used by running programs; its contents are generally lost when power is removed.
- **Storage:** retains files and programs, for example on an SSD or hard drive.
- **Motherboard:** connects the major components and provides communication pathways.
- **Power supply or battery:** provides electrical power.
- **Input/output devices:** allow the computer to receive input and present results.

The CPU contains components such as an arithmetic logic unit and control logic. The ALU performs arithmetic and logical operations, while control logic coordinates instruction execution. Registers hold small amounts of data used directly by the processor.

## 2. Memory and storage are different

RAM is designed for active work and is much faster than persistent storage, but it is volatile. An SSD or hard drive keeps data when the computer is powered off. When an application runs, the operating system loads the necessary code and data from storage into memory.

This difference matters during incident response. A disk image may preserve files and configuration, while a memory capture can contain running processes, network connections, and other volatile evidence. Each acquisition method has limitations and should be chosen according to the investigation.

## 3. The operating system

The operating system manages hardware resources and provides services to applications. It schedules processes, allocates memory, manages files, controls devices, and enforces access permissions.

Applications normally request services through system calls or operating-system APIs rather than directly controlling hardware. The kernel has more privileged access than ordinary user programs. A kernel vulnerability can therefore have a larger impact than a flaw restricted to one unprivileged application.

## 4. The boot process

When a computer starts, firmware performs early hardware initialisation and selects a boot device. A bootloader then loads the operating system kernel. The kernel initialises system resources and starts the user-space services required for normal operation.

Modern systems may use UEFI and Secure Boot to help ensure that only trusted boot components are loaded. Secure Boot is not a complete defence against every compromise, but it raises the difficulty of certain boot-level attacks when correctly configured.

## 5. Files, processes, and permissions

A process is a running instance of a program. Processes have identities and permissions that determine what resources they can access. Files and directories also have access controls. A process should not be able to read another user's private files or modify protected system components without appropriate permission.

Useful Linux inspection commands include:

- ps aux: lists processes.
- free -h: shows memory usage.
- df -h: reports filesystem space.
- ls -la: lists directory entries, including hidden files, with permissions.

These are read-only inspection examples. Their output depends on the system and should not be copied as if it came from the room.

## 6. Security implications

Keep firmware and operating systems updated, enable disk encryption where appropriate, use least privilege, lock unattended devices, and protect physical access. Secure Boot and endpoint monitoring can help, but neither replaces patching and good account management. Hardware disposal should also include secure handling of stored data.

## 7. Working through the room

Read each question and identify whether it refers to a hardware component, memory type, operating-system responsibility, or boot stage. Match the described job to the component that performs it. If the task shows a diagram, follow the labelled data path instead of relying on the component's name alone.

## What I took from this room

A computer is a stack of cooperating components. Understanding the CPU, memory, storage, operating system, and boot process makes both troubleshooting and security analysis more precise.