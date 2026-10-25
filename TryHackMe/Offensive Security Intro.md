# Offensive Security Intro: TryHackMe Walkthrough

Room: https://tryhackme.com/room/offensivesecurityintro

Offensive security tests systems from an attacker's perspective so weaknesses can be found before someone abuses them. The work must be authorised and scoped. The point is to provide evidence that helps the owner improve security, not to damage systems or access data without permission.

## 1. Understand the objective

An engagement may focus on a web application, host, network, or a particular security control. Before testing, identify the target and the permitted actions. In TryHackMe, use the assigned machine and the room's instructions rather than a random public target.

## 2. Reconnaissance and enumeration

Start by discovering what is exposed. For a lab host, a basic scan pattern is:

    nmap -sC -sV -oN scan.txt TARGET_IP

Replace TARGET_IP with the address provided by the room. This checks common ports, runs default scripts, and attempts service-version detection. Read the output and identify which services deserve further investigation.

For a web service, inspect the homepage, links, source, headers, and paths that the application exposes. Use a directory-discovery tool only if needed and only against the assigned target. An open port or a discovered directory is a clue, not a vulnerability by itself.

## 3. Validate a weakness

Use the evidence from enumeration to form a hypothesis. If a service version appears outdated, check the relevant advisory and the conditions needed for exploitation. If an application accepts input, determine how that input is processed and whether the server enforces the expected rules.

Change one thing at a time and record the response. Avoid running a destructive exploit just to see what happens. A safe proof should be sufficient to demonstrate the weakness without affecting unrelated data or service availability.

## 4. Understand impact

A vulnerability's impact depends on what an attacker can actually do. Reading another user's record, executing a command as a low-privilege account, and obtaining root access are different levels of impact. Verify the identity and permission context before reporting a result.

## 5. Learn from the result

A useful finding includes the affected component, evidence, preconditions, impact, and remediation. Fixes might involve patching software, enforcing server-side access control, removing exposed secrets, or reducing service permissions. Retest after the fix to confirm the original behaviour is no longer possible.

## 6. Working through the room

Follow the task's guided activity and pay attention to the difference between reconnaissance, exploitation, and defensive remediation. If a question asks what a tool does, explain its role in the workflow. If it asks for a value from the target, use the live lab output rather than an invented example.

## What I took from this room

Offensive security is a controlled way to discover weaknesses and prove their impact. Good testing requires permission, a methodical process, minimal disruption, and clear reporting that turns findings into fixes.