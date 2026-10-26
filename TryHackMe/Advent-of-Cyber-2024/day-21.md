# Advent of Cyber 2024 - Day 21: Reverse Engineering

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

HELP! I've been reverse engineered!

## Background

.NET applications compile into Common Intermediate Language (CIL) bytecode rather than native machine code. CIL retains high-level metadata (variable names, types, method structures), allowing decompilers like ILSpy and dnSpy to reconstruct source code nearly identical to original C# source.

Static reverse engineering of .NET malware quickly exposes command handlers, embedded strings, persistence routines, and network endpoints.

## Walkthrough

**Step 1 — Decompile the Malware in ILSpy**

Open `MayorMalware.exe` in ILSpy. Expand the assembly hierarchy and inspect the main namespace.

**Step 2 — Identify Key Execution Methods**

Review class methods:
- Method responsible for downloading payloads: `DownloadAndExecuteFile`
- Target process spawned to host malicious child code: `explorer.exe`

**Step 3 — Extract Hardcoded C2 Configuration**

Inspect string constants in the networking class to find hardcoded domain: `mayorc2.thm`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the method name responsible for fetching and executing payloads? | `DownloadAndExecuteFile` |
| What legitimate Windows binary does the malware spawn? | `explorer.exe` |
| What is the hardcoded C2 domain name? | `mayorc2.thm` |
