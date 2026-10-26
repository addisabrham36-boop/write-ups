# Advent of Cyber 2024 - Day 6: Malware Static Analysis

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

If I can't find a nice malware to use, I'll make my own — but first let's tear apart someone else's with static analysis.

## Background

Static malware analysis examines a binary without executing it. This is safer than dynamic analysis (running the sample) and often reveals a surprising amount of useful intelligence: file hashes for threat intel lookups, imported API functions that hint at capabilities, embedded strings pointing to C2 infrastructure, and PE header metadata showing compilation timestamps and target architecture.

PEStudio is a popular Windows tool for static PE analysis. It parses the Portable Executable format to surface imports (DLL functions the malware uses), exports, strings, indicators (blacklisted functions, suspicious entropy), and VirusTotal results. Looking at imports like `VirtualAlloc`, `CreateRemoteThread`, and `WriteProcessMemory` immediately signals process injection capabilities.

YARA rules are pattern-matching signatures for malware. A YARA rule defines strings (text or hex byte sequences) and a condition combining them. Security teams write YARA rules to hunt for known malware families across file systems, memory dumps, and network captures. Understanding the binary deeply enough to write a YARA rule for it is the goal of solid static analysis.

## Walkthrough

**Step 1 — Hash the sample**

Before anything else, get the SHA256 hash for threat intelligence lookups:

```bash
sha256sum malware_sample
```

Or in PowerShell:

```powershell
Get-FileHash malware_sample -Algorithm SHA256
```

Result:

```
F29C270068F865EF4A747E2683BFA07667BF64E768B38FBB9A2750A3D879CA33
```

**Step 2 — Load in PEStudio**

Open the sample in PEStudio. Review:
- **Indicators tab** — flagged suspicious characteristics
- **Imports tab** — DLL functions imported (look for injection-related APIs)
- **Strings tab** — embedded text, URLs, registry keys, commands

**Step 3 — Review suspicious imports**

Key imports to flag:

| Import | Significance |
|---|---|
| `VirtualAlloc` | Allocate executable memory (shellcode staging) |
| `CreateRemoteThread` | Inject code into another process |
| `WriteProcessMemory` | Write shellcode into a target process |

**Step 4 — Write a YARA rule**

Create a detection rule based on distinctive strings or byte patterns found in the binary:

```yara
rule MayorMalware_Day6 {
    meta:
        description = "Detects AoC 2024 Day 6 malware sample"
        sha256 = "F29C270068F865EF4A747E2683BFA07667BF64E768B38FBB9A2750A3D879CA33"
    strings:
        $s1 = "VirtualAlloc" ascii
        $s2 = "CreateRemoteThread" ascii
    condition:
        all of them
}
```

**Step 5 — Scan with YARA**

```bash
yara rule.yar malware_sample
```

A match confirms our rule works.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the SHA256 hash of the malware sample? | `F29C270068F865EF4A747E2683BFA07667BF64E768B38FBB9A2750A3D879CA33` |
