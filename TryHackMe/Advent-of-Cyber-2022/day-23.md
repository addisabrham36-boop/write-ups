# Advent of Cyber 2022 - Day 23: Defense in Depth: Mission ELFPossible

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Overview & Scenario
Implementing multi-layered defense architecture (Defense in Depth) so that failure of a single security control does not compromise the crown jewels.

## Learning Objectives & Key Concepts
+ Understand the concept of Defense in Depth
+ Differentiate between single-layer and multi-layer defensive architectures
+ Analyze security controls protecting Santa's Vault

### Technical Context
Defense in Depth is an information assurance concept in which multiple layers of security controls (defense mechanisms) are placed throughout an IT system. Its intent is to provide redundancy in the event a security control fails or a vulnerability is exploited.

## Walkthrough & Investigation

**Case 1 — Single Layer of Defense**
In the first architecture scenario, Santa's Vault is protected solely by network perimeter credentials. Accessing the web portal and submitting the weak administrator password `S3cr3tV@ultPW` grants immediate, unhindered access to the vault records, disclosing the Case 1 flag.

**Case 2 — Multi-Layer Defense Architecture**
In the second hardened architecture, Santa's team implements layered controls:
1. **Network Layer:** Strict firewall rules restricting incoming traffic to internal administrative subnets.
2. **Identity & Access Management:** Multi-factor verification requiring knowledge of Santa's favorite snack: `MilkAndCookies`.
3. **Application & Vault Layer:** A high-entropy credential scheme requiring `3XtrR@_S3cr3tV@ultPW` to decrypt the primary database ledger.

Attempting to bypass single barriers fails because subsequent controls enforce verification, successfully preventing lateral movement and unauthorized exfiltration.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the Case 1 password for Santa's Vault? | `S3cr3tV@ultPW` |
| What is the Case 1 Flag? | `flag{hidden}` |
| What is Santa's favourite thing in Case 2? | `MilkAndCookies` |
| What is the Case 2 password for Santa's Vault? | `3XtrR@_S3cr3tV@ultPW` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
