# The Hacker Methodology — TryHackMe

Room: https://tryhackme.com/room/hackermethodology

A methodology gives a security tester a repeatable way to approach a target. Without one, it is easy to jump between tools, miss important evidence, or repeat work that has already been done.

## A practical sequence

I start by confirming scope and gathering information. Next, I enumerate the exposed services and applications. I use those findings to identify likely weaknesses, then validate relevant issues in a controlled way. Finally, I document the evidence, impact, and remediation.

The process is iterative. A new finding can change what I need to investigate, and an assumption may turn out to be wrong. That is normal, as long as I update my notes instead of pretending the original guess was correct.

## Evidence and reporting

For every step, I should record the command or action, the important output, and why it matters. A scanner's result is a lead rather than a final conclusion. The strongest findings explain what was tested, what happened, and how another person can reproduce it safely.

## What I took from this room

Methodology keeps testing organised and defensible. It helps me move from reconnaissance to validation without losing track of the evidence, and it makes the final report useful to the people responsible for fixing the system.