# Inside a Computer System: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/insideacomputer

## Room Overview & Information

**Category:** Computer fundamentals  
**Difficulty:** Beginner  
**Focus:** Computer components, their functions, and the startup sequence from power-on to operating-system handoff.

Before securing a computer, it helps to understand the hardware and firmware that make it work. This room uses a castle analogy and two interactive static sites: one for identifying PC components and another for stepping through the boot process.

## Task 1: Introduction

The room's main idea is that you cannot effectively defend a system if you do not understand its components and how they interact. You need to know where information lives, how users and devices connect, and what controls the system's startup.

### Question: “Why does the room say you must understand a computer before securing it?”

**Answer:** Defending a system you do not understand is like defending a castle you have never seen. You need to understand the layout, how people and data enter, and who can access or change important resources.

## Task 2: Identify PC Components

Open the component-identification exercise using **View Site**. Match each component to its role.

### Question: “What is the motherboard compared to in the human-body analogy, and what does it do?”

**Answer:** It is compared to the skeleton and nervous system. It holds components in place and connects them through the CPU socket, RAM slots, expansion slots, and ports.

### Question: “What does the CPU do, and how do modern CPUs improve throughput?”

**Answer:** The CPU executes instructions. Modern CPUs have multiple cores that can process instructions in parallel. The CPU connects through a socket on the motherboard.

### Question: “What is RAM, and why is it called volatile?”

**Answer:** RAM is short-term working memory that holds data the CPU needs quickly. It is volatile because its contents are lost when power is removed.

### Question: “How do HDDs and SSDs differ as long-term storage?”

**Answer:** HDDs use moving mechanical parts and are generally slower but can offer high capacity at low cost. SSDs use memory chips and have no moving parts, so they are typically faster. Both can connect through SATA or PCI Express, depending on the device.

### Question: “What is the role of a network adapter?”

**Answer:** It allows a computer to communicate with other systems over a wired or wireless network. It may be built into the motherboard or added as an expansion card.

### Question: “Why must the PSU be sized carefully for a system?”

**Answer:** The power supply unit provides energy to all components. If it cannot supply enough power for the system's needs, the computer may fail or become unstable.

### Question: “What does a graphics card do, and how does it usually connect?”

**Answer:** A graphics card processes visual data from the operating system and applications for display on a monitor. A discrete graphics card commonly connects through a PCI Express slot.

### Question: “Give examples of input devices, output devices, and common connectors.”

**Answer:**
- **Input:** keyboard, microphone, mouse, scanner.
- **Output:** monitor, printer, speakers.
- **Connectors:** USB, HDMI, DisplayPort.

## Task 3: Follow the Boot Sequence

Open the boot-sequence exercise using **View Site** and step through each stage. The purpose is to understand how the computer starts hardware, checks it, chooses a boot device, and transfers control to the operating system.

### Question: “What happens in Step 1 when you press the power button?”

**Answer:** A signal is sent to the power supply unit to allow power to flow through the system.

### Question: “What firmware manages component startup, and how does BIOS relate?”

**Answer:** UEFI, the Unified Extensible Firmware Interface, manages startup on modern systems. BIOS performs a similar role but is the older firmware model.

### Question: “What is POST (Power-On Self Test)?”

**Answer:** POST checks that required components are present, configured correctly, and functioning sufficiently for startup to continue.

### Question: “How does the system choose where to boot from?”

**Answer:** UEFI uses an ordered boot-priority list. It checks the configured devices in order for a valid boot routine.

### Question: “What does the bootloader do in the final boot step?”

**Answer:** The bootloader starts the operating-system loading process and loads the OS into memory. Firmware then hands control to the operating system.

## Task 4: Interactive flags

The room displays a flag after each interactive exercise is completed:
- Identifying all PC components.
- Completing the boot-sequence simulation.

Retrieve both values from the active room and submit them. They are intentionally redacted in this public write-up as `THM{hidden}`.

## Component reference

| Component | Main role | Security relevance |
|---|---|---|
| Motherboard | Connects hardware components | Firmware and device interfaces affect the platform's trust boundary |
| CPU | Executes instructions | CPU vulnerabilities can affect isolation and execution |
| RAM | Holds active data | Volatile memory may contain secrets and forensic evidence |
| SSD/HDD | Persistent storage | Stores OS files, application data, logs, and credentials |
| Network adapter | Connects to networks | Exposes the system to network traffic and remote services |
| PSU | Supplies electrical power | Stable power is required for reliable operation |
| Graphics card | Processes graphics workloads | Can also execute general-purpose compute tasks |
| Input/output devices | Let users interact with the computer | Input devices can be abused for social engineering or hardware attacks |

## Boot sequence reference

1. The power button signals the system to start.
2. The power supply provides power to the components.
3. UEFI firmware starts.
4. POST checks key hardware components.
5. UEFI selects a boot device based on configured priority.
6. The bootloader begins loading the operating system into RAM.
7. Control passes to the operating system.

Exact implementation details vary by device and firmware, but the room's sequence provides the basic mental model.

## Key takeaways

The motherboard connects components, the CPU executes instructions, RAM holds active data, storage retains data, and the network adapter enables communication. During startup, firmware checks the system, selects a boot device, and launches the bootloader. These basics help security analysts understand hardware trust, boot integrity, and where evidence may exist.

## Expanded practical study guide

### 1. CPU, memory, and storage

The CPU executes instructions. RAM holds working data for active processes and is normally volatile, while persistent storage retains data across shutdowns. The operating system coordinates access to these resources and isolates processes from one another.

A CPU core can execute instructions, and modern processors use caches to reduce the time needed to access frequently used data. The operating system schedules runnable threads, handles interrupts, and provides system calls so applications can request privileged services safely.

### 2. Input, output, and buses

Input devices provide data to the system; output devices present results. Storage, network, display, and peripheral controllers communicate through buses and device interfaces. A device driver lets the operating system interact with a particular device.

Hardware faults can resemble software problems. When diagnosing an issue, record the symptoms and determine whether they occur before the operating system loads, only in one application, or across multiple applications.

### 3. Operating-system responsibilities

The OS manages processes, memory, filesystems, device access, users, permissions, networking, and logs. User applications normally run with limited privileges. The kernel mediates sensitive operations, helping prevent one process from directly controlling another process's memory or system resources.

### 4. Files and data representation

Files contain bytes interpreted according to a format. The extension is a naming convention, not proof of content. Use `file` to identify likely formats and a suitable parser to inspect them. Never execute an unknown file merely to learn what it does.

### 5. Security implications

Least privilege limits the damage caused by a compromised account or application. Secure boot and code-signing mechanisms can help establish trust in the boot chain and software, while updates fix known vulnerabilities. Disk encryption protects data at rest when implemented and keyed correctly, but it does not protect data from an already-authorized process running on an unlocked system.

### 6. Answering room tasks

Map each question to the component or responsibility it describes: CPU execution, volatile memory, persistent storage, device I/O, kernel services, or application behaviour. Use the exact terminology in the room and distinguish a hardware component from the operating-system function that manages it.

### 7. Review and troubleshooting

When a computer behaves unexpectedly, change one variable at a time and note when the symptom occurs. Check system logs, resource usage, device status, and recent changes. Do not assume every performance issue is a malware incident or every crash is a hardware failure.

### 8. Key takeaway

A computer system is a coordinated set of hardware and software layers. Understanding the responsibility of each layer helps explain performance, reliability, and security problems without jumping to unsupported conclusions.
