# Offensive Security Intro: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/offensivesecurityintro

## Room Overview & Information

**Category:** Offensive security fundamentals  
**Difficulty:** Easy  
**Objective:** Understand authorised offensive security and complete the simulated vulnerable-bank interaction.

This is an interactive introduction rather than a boot-to-root target. There is no real bank, no shell, and no privilege-escalation stage. The practical demonstrates how an intentionally scripted website responds when a user submits the required answer.

## Phase 1: Understand Offensive Security

Offensive security uses attacker-style testing to discover and validate security weaknesses before they are abused by criminals. Examples include penetration testing, red teaming, and vulnerability research. A professional test has explicit permission, a defined scope, safety rules, evidence collection, and a report that helps the owner remediate findings.

The distinction between offensive security and unauthorised hacking is not the tool being used. It is the permission, scope, and conduct of the activity.

## Phase 2: Complete the simulated bank task

### 1. Open the lab site

Open the room's **View Site** link. This launches the fictional banking website used by the practical. Read the prompt, inspect the interactive controls, and do not navigate to a real bank site.

### Question: “Which of the following options better represents the process where you simulate a hacker's actions to find vulnerabilities in a system?”

**Answer:** **Offensive Security**.

The question asks for the professional discipline that simulates attacker behaviour to discover weaknesses. The aim is to identify issues in a controlled, authorised manner so they can be fixed before a real attack.

### 2. Verify the simulated result

After submitting the correct answer, the page displays a message above the account balance.

### Question: “Above your account balance, you should now see a message indicating the answer to this question. Can you find the answer you need?”

**Answer:** **BANK-HACKED**.

This is the text deliberately displayed by the simulation. It is not evidence that a real banking system was compromised.

## Phase 3: How to Document an Offensive Test

Although the room is very simple, a professional record should still separate action from result:

1. **Objective:** identify what the task asks you to demonstrate.
2. **Scope:** use only the site deployed by the room.
3. **Action:** submit the room's required input.
4. **Evidence:** record the output displayed by the page.
5. **Interpretation:** explain what the application showed and what it did not prove.
6. **Cleanup:** stop when the task is complete and avoid interacting with unrelated services.

The displayed message proves only that the simulated task accepted the answer. It does not prove a general vulnerability exists in an external system.

## Phase 4: Question and Answer Summary

| Exact question | Answer |
|---|---|
| Which option represents simulating a hacker's actions to find vulnerabilities? | **Offensive Security** |
| What message appears above the account balance after the correct interaction? | **BANK-HACKED** |

## Phase 5: Key Takeaways & Remediation

Offensive security is an authorised process for identifying weaknesses. A useful test should produce reproducible evidence, explain impact, and recommend a fix. In a real assessment, confirm permission and scope before testing, use the least disruptive method that proves the issue, protect any sensitive data encountered, and report findings clearly.

The practical's lesson is about the method and its boundaries, not about attacking a real bank.