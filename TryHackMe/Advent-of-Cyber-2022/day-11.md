# Advent of Cyber 2022 - Day 11: Memory Forensics

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Analyzing a Windows memory dump (`workstation.vmem`) using **Volatility 3** to detect running rogue processes, suspicious network connections, and extract malware binaries.

## Walkthrough
1. Determine memory image OS profile:
   ```bash
   python3 vol.py -f workstation.vmem windows.info
   ```
   OS version is identified as Windows `10`.
2. Enumerate active processes:
   ```bash
   python3 vol.py -f workstation.vmem windows.pslist
   ```
   A suspicious process named `mysterygift.exe` is identified running under PID `2040`.
3. Dump files associated with PID 2040:
   ```bash
   python3 vol.py -f workstation.vmem windows.dumpfile --pid 2040
   ```
   Volatility successfully extracts `16` memory-mapped files and DLL handles.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the Windows version number of the memory image? | `10` |
| What is the name of the binary that secret Santa left? | `mysterygift.exe` |
| What is the Process ID (PID) of this binary? | `2040` |
| Dump the contents of this binary. How many files are dumped? | `16` |
