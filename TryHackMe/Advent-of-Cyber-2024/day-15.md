# Advent of Cyber 2024 - Day 15: Active Directory

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

I think it's time to wrap up!

## Background

Active Directory (AD) is the identity backbone of enterprise Windows environments. Domain Controllers record extensive telemetry in the Windows Security Event Log.

Incident responders investigate key event IDs:
- **Event ID 4624:** Successful account logon.
- **Event ID 4625:** Failed logon attempt.
- **Event ID 4688:** New process created.
- **Event ID 4720:** User account created.
Adversaries frequently manipulate Group Policy Objects (GPOs) to maintain persistent domain-wide access.

## Walkthrough

**Step 1 — Audit Security Event Logs**

Open Event Viewer and filter the Security log for Event ID 4624:
Correlating timestamps shows suspicious logon activity on `07/11/2024`.

**Step 2 — Enumerate Active Directory with PowerShell**

Inspect console history to see commands executed by the intruder:
```powershell
Get-ADUser -Filter * -Properties MemberOf | Select-Object Name
```
Searching configuration scripts reveals a hardcoded password: `SuperSecretP@ssw0rd!`.

**Step 3 — Detect Malicious Group Policy**

Inspect modified GPOs across the domain:
```powershell
Get-GPO -All | Select-Object DisplayName
```
Identified rogue persistence GPO: `Malicious GPO - Glitch_Malware Persistence`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| On what date was the initial malicious logon detected? | `07/11/2024` |
| What Windows Security Event ID tracks successful logon attempts? | `4624` |
| What PowerShell command was run to enumerate AD users? | `Get-ADUser -Filter * -Properties MemberOf | Select-Object Name` |
| What is the recovered domain password? | `SuperSecretP@ssw0rd!` |
| What is the name of the malicious Group Policy Object? | `Malicious GPO - Glitch_Malware Persistence` |
