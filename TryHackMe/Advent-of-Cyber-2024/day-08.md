# Advent of Cyber 2024 - Day 8: Shellcode

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

Shellcodes of the world, unite!

## Background

Shellcode consists of position-independent machine code instructions executed directly by the processor. In penetration testing and malware development, shellcode is commonly injected into memory to spawn reverse shells or stage secondary payloads.

PowerShell provides access to low-level Windows APIs via the .NET runtime and `P/Invoke`. Attackers abuse functions such as `VirtualAlloc` (allocating memory), `Marshal.Copy` (copying shellcode into memory), and `CreateThread` (launching an execution thread) to run shellcode entirely in memory, evading traditional disk-based antivirus scanners.

## Walkthrough

**Step 1 — Analyze the PowerShell Stager**

Inspect the malicious PowerShell script used to deliver the payload:
```powershell
Get-Content -Path .\dropper.ps1
```
The script allocates unmanaged memory using `.NET Marshal`:
```powershell
$mem = [System.Runtime.InteropServices.Marshal]::AllocHGlobal($shellcode.Length)
[System.Runtime.InteropServices.Marshal]::Copy($shellcode, 0, $mem, $shellcode.Length)
```

**Step 2 — Inspect Memory Protection Changes**

Before executing the injected buffer, the script calls `VirtualProtect` to mark the allocated page as executable (`PAGE_EXECUTE_READWRITE` / `0x40`).

**Step 3 — Extract and Disassemble Shellcode**

Export the embedded byte array into a binary file:
```bash
cat shellcode.hex | xxd -r -p > shellcode.bin
scdbg -f shellcode.bin
```
Emulation reveals API resolution and reverse shell connection attempts.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What PowerShell method is used to allocate unmanaged memory? | `AllocHGlobal` |
| What Windows API function changes memory page protection? | `VirtualProtect` |
| What is the flag retrieved from the lab? | `THM{SHELLCODE_EXECUTION_SUCCESS}` |
