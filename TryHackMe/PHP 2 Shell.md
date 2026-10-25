# PHP 2 Shell — TryHackMe

Room: https://tryhackme.com/

This draft covers the general idea behind PHP-to-shell vulnerabilities, but I need to verify the exact room URL and task sequence before claiming room-specific results.

## The basic idea

PHP applications sometimes invoke operating-system commands or pass user-controlled values into functions that can execute commands. If input is not handled safely, an attacker may be able to influence what the server runs. The precise risk depends on the function, escaping, execution context, and system permissions.

Defensive code should avoid building shell commands from untrusted input. Where possible, use safer language APIs, validate inputs against a strict allowlist, and run services with the least privileges they need.

## Lab approach

In an authorised challenge, I should first identify the application behaviour and the relevant input. Then I should test only the payloads and methods allowed by the room, observing the response and confirming what the server actually did. A suspicious error message alone is not proof of command execution.

## What I took from this topic

Command execution flaws can turn a small input-handling mistake into a serious server compromise. A good write-up should explain the vulnerable input, the evidence of execution, and the fix, without pretending a generic payload works on every PHP application. Exact room-specific steps need to be added after confirming the challenge details.