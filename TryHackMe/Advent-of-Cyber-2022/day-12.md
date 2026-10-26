# Advent of Cyber 2022 - Day 12: Malware Analysis

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Static and dynamic triage of a compiled PE executable using **Detect It Easy (DIE)**, **Capa**, and **Sysinternals Process Monitor (ProcMon)**.

## Walkthrough
1. Load `mysterygift.exe` into Detect It Easy:
   - Architecture: `64-bit`
   - Packer detected: `upx`
   - Compiler / Linker: `nim`
2. Run ProcMon while executing the unpacked sample in an isolated VM.
3. Observe file system activity:
   - Malware creates a persistence batch file under the Windows Startup directory:
     `C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\wishes.bat`
   - Malware drops two files in `C:\Users\Administrator\`: `test.jpg` and `wishes.bat`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the architecture of the malware sample? | `64-bit` |
| What packer is used in the malware sample? | `upx` |
| What compiler was used to build the malware? | `nim` |
| Path of the startup script created for persistence? | `C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\wishes.bat` |
| Two files created under `C:\Users\Administrator\` (alphabetical)? | `test.jpg,wishes.bat` |
