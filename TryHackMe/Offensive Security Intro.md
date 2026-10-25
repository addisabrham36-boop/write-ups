# Offensive Security Intro: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/offensivesecurityintro

## Room Overview & Information

**Category:** Offensive security fundamentals  
**Difficulty:** Easy  
**Objective:** Learn what offensive security means and use the room's simulated bank website to see a basic authorised attack in action.

This room introduces offensive security through a deliberately vulnerable, fictional bank. The practical is a simulation, not a real banking service. Only interact with the website deployed by TryHackMe.

## Task 1: Hacking your first website

Open the room's **View Site** link to launch the simulated bank website. Review the page and locate the interactive question. The goal is to connect the general definition of offensive security to the activity shown by the lab.

### Question: “Which of the following options better represents the process where you simulate a hacker's actions to find vulnerabilities in a system?”

**Answer: Offensive Security.**

Offensive security uses authorised attacker-style testing to find and validate weaknesses before a real attacker exploits them. The work should stay within the agreed scope and should produce evidence and recommendations.

After submitting the correct answer, look above the account balance on the simulated bank page. The site displays a message indicating that the exercise succeeded.

### Question: “Above your account balance, you should now see a message indicating the answer to this question. Can you find the answer you need?”

**Answer: BANK-HACKED**

This is the message displayed by the room's simulation. It is not a real bank compromise; it is a deliberately scripted learning exercise.

## How the exercise works

The room uses a simple web interface to show that an application can reveal information after a particular action. In a real assessment, the tester would inspect the request and response, identify what control failed, and verify the impact without accessing unrelated data.

A useful process is:
1. Read the task and define the lab scope.
2. Inspect the page and the available interaction.
3. Submit the answer requested by the room.
4. Read the message displayed by the simulated application.
5. Record what the application demonstrated.
6. Stop once the task is complete.

## Offensive security versus unauthorised hacking

The techniques may look similar, but authorisation and scope are what distinguish a penetration test from unauthorised activity. A professional tester has permission, follows rules of engagement, minimises impact, preserves evidence, and reports the findings so the owner can fix them.

## Key takeaways

Offensive security simulates attacker behaviour to find weaknesses in a controlled and authorised way. The room's answer is **Offensive Security**, and the simulated site displays **BANK-HACKED** after the correct interaction. The lesson is about the method and its boundaries, not about attacking a real bank.

## Expanded room workflow

### 1. Understand the legal boundary

Offensive security uses attacker techniques to discover weaknesses under explicit authorization. The scope defines the hosts, applications, methods, time window, and prohibited actions. A public IP or accessible login page is not permission to test it.

### 2. Follow a repeatable methodology

A controlled assessment typically proceeds through scoping, reconnaissance, enumeration, vulnerability validation, exploitation where permitted, impact assessment, cleanup, and reporting. Not every task requires every phase. Avoid running an exploit before understanding the service and the risk.

### 3. Reconnaissance and enumeration

Passive reconnaissance gathers information from public sources. Active reconnaissance sends traffic to the target. Enumeration identifies concrete services, users, shares, routes, or application behaviour. Keep a record of commands, timestamps, and outputs so findings can be reproduced.

### 4. Validate a vulnerability

A version number or banner is only a lead. Check whether the affected feature is enabled, whether the target configuration is vulnerable, and whether the proof of concept applies. Prefer a minimal, non-destructive validation and stop once the task objective is demonstrated.

### 5. Initial access and privilege boundaries

Initial access means obtaining a foothold under the permissions granted by the vulnerable path. Privilege escalation means increasing those permissions. Verify the current identity before and after a step with the appropriate command. Do not call a user shell root access unless the effective identity confirms it.

### 6. Report and remediate

A useful report contains the affected asset, steps to reproduce within scope, evidence, impact, severity rationale, and specific remediation. Remove temporary files, restore lab changes when required, and do not expose credentials or flags in public notes.

### 7. Complete the room tasks

Read the scenario and identify which phase or role the question describes. Use the official room's definitions where exact wording matters. This room is an introduction, so the goal is to understand the workflow and ethics rather than to force an exploit where none is required.
