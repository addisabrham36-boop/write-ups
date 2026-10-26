# Advent of Cyber 2022 - Day 7: CyberChef

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Using CyberChef ("The Cyber Swiss Army Knife") to extract and deobfuscate malicious URLs, Base64 payloads, and VBA strings from weaponized office documents (maldocs).

## Walkthrough
1. Access CyberChef on the VM (`http://localhost:8080` or desktop shortcut). The version displayed is `9.49.0`.
2. Load the suspicious macro code into the **Input** pane.
3. Build the CyberChef pipeline recipe:
   - `Strings` (extract printable sequences)
   - `Find / Replace` (remove string obfuscation)
   - `Drop bytes`
   - `Defang URL` (replace `http://` with `hxxp://` and `.` with `[.]`)
4. Inspecting the defanged URLs yields `hxxps[://]cdn[.]bandityeti[.]THM/files/index/` and an internal customer support ticket containing `THM_MYSTERY_FLAG`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the version of CyberChef found in the attached VM? | `9.49.0` |
| What is the last URL extracted (defanged format)? | `hxxps[://]cdn[.]bandityeti[.]THM/files/index/` |
| What is the ticket found in one of the domains? | `THM_MYSTERY_FLAG` |
