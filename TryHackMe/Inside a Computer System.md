# Inside a Computer System

{% embed url="https://tryhackme.com/room/insideacomputersystem" %}

A foundational exploration of computer hardware architecture, comparing physical PC components to human anatomy, understanding bus topologies, and mapping the system boot sequence.

---

## 1. Hardware Building Blocks

- **CPU (Central Processing Unit):** The "brain" executing arithmetic, logic, and control instructions.
- **RAM (Random Access Memory):** Volatile, high-speed memory holding data and program code actively utilized by the CPU.
- **Storage (HDD / SSD):** Non-volatile secondary storage preserving user files and operating system files across power cycles.
- **Motherboard:** Main printed circuit board establishing bus interconnects (PCIe, SATA, RAM channels) between components.
- **PSU (Power Supply Unit):** Converts alternating current (AC) wall power into regulated low-voltage direct current (DC).

---

## 2. Boot Sequence (POST to OS)

1. **Power-On:** PSU sends a "Power Good" signal to the motherboard.
2. **BIOS / UEFI Initialization:** Firmware executes Power-On Self-Test (POST) to verify hardware integrity.
3. **Bootloader Execution:** BIOS/UEFI searches configured boot devices for the boot sector / EFI partition.
4. **Kernel Hand-off:** The bootloader loads the operating system kernel into RAM and transfers execution control.

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Concept** | Primary processor performing calculations | `CPU` |
| **Concept** | High-speed volatile memory for active processes | `RAM` |
| **Concept** | Non-volatile persistent data storage | `HDD / SSD` |
| **Concept** | Firmware executing POST diagnostic checks on boot | `BIOS / UEFI` |
| **Concept** | Software utility loading kernel into RAM | `Bootloader` |
