# Offensive Security Intro — TryHackMe

Room: https://tryhackme.com/room/offensivesecurityintro

Offensive security means testing systems from an attacker's perspective so that weaknesses can be found and fixed. The legal and ethical boundary matters: I should only test systems that I own or have explicit permission to assess.

## The basic process

A simple engagement may include reconnaissance, enumeration, vulnerability analysis, controlled exploitation, and reporting. Reconnaissance collects information. Enumeration investigates discovered services in more detail. Vulnerability analysis looks for weaknesses that may apply to those services. Exploitation demonstrates impact within the agreed scope.

The order is not always perfectly linear. New information can send the tester back to an earlier step. The goal is to work from evidence instead of trying random attacks and hoping something works.

## Why reporting matters

A useful finding explains the affected asset, the evidence, the potential impact, and a practical recommendation. Saying “the server is insecure” is not enough for someone who has to fix it. The report should make the problem reproducible without exposing unnecessary sensitive information.

## What I took from this room

Offensive security is not simply breaking things. It is a controlled process for identifying and demonstrating weaknesses so the owner can improve security. Strong fundamentals in networking, Linux, Windows, and web applications make the testing more reliable and the report more useful.