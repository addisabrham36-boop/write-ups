# Introduction to Operating Systems

{% embed url="https://tryhackme.com/room/introductiontooperatingsystems" %}

An introduction to operating system duties (process scheduling, memory virtualization, file systems, device drivers) and the architectural boundary between user space and kernel space.

---

## 1. Core Functions of an Operating System

An Operating System (OS) serves as the mediator between physical hardware and end-user software:
- **Process Management:** Allocating CPU time slices via thread scheduling.
- **Memory Management:** Providing virtual memory abstraction to isolate processes.
- **File System Management:** Structuring data storage hierarchies (ext4, NTFS).
- **Device Management:** Abstracting peripheral hardware via device drivers.

---

## 2. Kernel Space vs User Space

```
+---------------------------------------------------+
|               User Space (Ring 3)                 |
|   Web Browsers, Text Editors, Shells, Utilities   |
+---------------------------------------------------+
                         |
                 System Calls (syscall)
                         |
+---------------------------------------------------+
|               Kernel Space (Ring 0)               |
|   Core Kernel, Hardware Drivers, Memory Manager   |
+---------------------------------------------------+
```

- **Ring 3 (User Space):** Unprivileged environment where user applications execute. Crashes here do not bring down the entire system.
- **Ring 0 (Kernel Space):** Privileged ring with unrestricted access to memory and physical hardware instructions.

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Concept** | Core software layer managing hardware and applications | `Operating System` |
| **Concept** | Privileged execution space interacting directly with hardware | `Kernel Space` |
| **Concept** | Unprivileged execution ring where user programs run | `User Space` |
| **Concept** | Interface bridge transitioning execution from user to kernel mode | `System Calls` |
