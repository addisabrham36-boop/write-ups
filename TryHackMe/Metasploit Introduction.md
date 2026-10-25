# Metasploit Introduction — TryHackMe

Room: https://tryhackme.com/room/metasploitintro

Metasploit is a framework used for security testing. It includes modules for scanning, exploiting known vulnerabilities in authorised environments, and running supporting tasks. The framework makes workflows more organised, but it does not remove the need to understand the target or the module being used.

## Modules and the console

Metasploit modules have different purposes, including auxiliary checks, exploits, payloads, and post-exploitation tasks. The console lets me search for modules, inspect their options, and configure a module for a lab target.

A typical learning workflow is to search for a relevant module, read its description, inspect required options, and verify that the target and version match the module's assumptions. I should never treat a module name as proof that the target is vulnerable.

## Configuration matters

The target address, target port, and any required local callback settings must match the lab setup. A mistake in these values can cause the module to fail or send traffic to the wrong place. In an authorised room, I should use the target information supplied by TryHackMe and avoid carrying lab commands over to public systems.

## What I took from this room

Metasploit is a toolset, not a replacement for networking knowledge or vulnerability analysis. I need to understand what a module does, why it applies, and what its options mean. Controlled testing and clear notes are more important than getting a shell just to say that I got one.