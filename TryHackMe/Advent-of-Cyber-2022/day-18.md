# Advent of Cyber 2022 - Day 18: Threat Detection

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Writing and verifying **Sigma Rules**—a generic, open signature format for detecting adversary activity within SIEM logs.

## Walkthrough
1. Review the scenario: Bandit Yeti created a rogue local administrator account.
2. Draft a Sigma rule matching Windows Event ID 4720 (A user account was created):
   ```yaml
   title: Rogue Account Created by Yeti
   status: experimental
   logsource:
     product: windows
     service: security
   detection:
     selection:
       EventID: 4720
       TargetUserName|contains: 'BanditYeti'
     condition: selection
   ```
3. Running the Sigma verification engine reveals the newly created account `BanditYetiMini` and returns the Challenge 1 flag: `THM{n0t_just_your_u$ser}`.
4. In Challenge 2, writing a rule to detect unauthorized service installations yields `THM{wh@t_1s_Runn1ng_H3r3}`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the Challenge #1 flag? | `THM{n0t_just_your_u$ser}` |
| From the Challenge 1 log, what user account was created? | `BanditYetiMini` |
| What is the Challenge #2 flag? | `THM{wh@t_1s_Runn1ng_H3r3}` |
