# Advent of Cyber 2023 - Day 13: Intrusion Detection

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Applying the **Diamond Model of Intrusion Analysis**, threat hunting, and deploying defensive controls with honeypots and firewalls (`ufw`).

## Walkthrough
1. Diamond Model four core vertices: **Adversary**, **Capability**, **Infrastructure**, **Victim**.
2. Defensive strategy employs **Honeypot and Firewall** coordination.
3. Configure UFW firewall to block adversarial subnets:
   ```bash
   sudo ufw deny from 10.10.100.5 to any
   ```
4. Inspecting honeypot interaction logs reveals adversary attempts and unlocks flag: `THM{P0T$_W@11S_4_S@N7@}`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Which security model is used to analyse the breach? | `Diamond Model` |
| Defence capability used to actively search for malicious activity? | `Threat Hunting` |
| Main two infrastructure focuses? | `Honeypot and Firewall` |
| Firewall command used to block traffic? | `deny` |
| Flag in the honeypot story? | `THM{P0T$_W@11S_4_S@N7@}` |
