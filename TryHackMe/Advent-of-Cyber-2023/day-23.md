# Advent of Cyber 2023 - Day 23: Authentication Attacks

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Windows domain authentication security, NTLM relay attacks, and credential capturing using **Responder**.

## Walkthrough
1. Authentication protocols in Active Directory:
   - Ticket-based: **Kerberos**
   - Hash/challenge-response based: **NTLMv2**
2. Launch Responder on the local network interface:
   ```bash
   sudo responder -I eth0 -dwv
   ```
3. Coerce authentication from an internal host to capture the NetNTLMv2 challenge-response hash.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| AD authentication protocol that uses tickets? | `Kerberos` |
| AD authentication protocol that uses NTLM hash? | `NTLMv2` |
| Tool that can intercept authentication challenges? | `Responder` |
