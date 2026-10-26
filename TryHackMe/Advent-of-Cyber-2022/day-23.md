# Advent of Cyber 2022 - Day 23: Defense in Depth

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Implementing multi-layered defense architecture (Defense in Depth) so that failure of a single security control does not compromise the crown jewels.

## Walkthrough
- **Case 1 (Single Layer):** Direct access to Santa's Vault protected only by a single weak password (`S3cr3tV@ultPW`). Bypassing it reveals `THM{EZ_fl@6!}`.
- **Case 2 (Multi-Layer Defense):** Access requires multiple controls:
  - Network segmentation
  - Host identity verification
  - Santa's favorite thing: `MilkAndCookies`
  - High-entropy vault credential: `3XtrR@_S3cr3tV@ultPW`
- Clearing all defensive barriers demonstrates resilient system architecture.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Case 1 password for Santa's Vault? | `S3cr3tV@ultPW` |
| Case 1 Flag? | `THM{EZ_fl@6!}` |
| Case 2 Santa's favourite thing? | `MilkAndCookies` |
| Case 2 password for Santa's Vault? | `3XtrR@_S3cr3tV@ultPW` |
