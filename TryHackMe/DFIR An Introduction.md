# DFIR: An Introduction — TryHackMe

Room: https://tryhackme.com/room/introductoryroomdfir

DFIR stands for Digital Forensics and Incident Response. The two areas work together: forensics helps explain what happened using digital evidence, while incident response focuses on containing and recovering from the incident.

## Incident response

A response often includes preparation, identification, containment, eradication, recovery, and lessons learned. Real incidents do not always follow those stages in a neat line, but the structure helps teams avoid skipping important work.

Containment limits further damage. Eradication removes the cause or attacker access where possible. Recovery brings services back carefully and checks that they are operating normally. Afterward, the team reviews what happened and improves controls.

## Digital evidence

Evidence can include system logs, event logs, memory captures, disk images, browser artefacts, network captures, and files left by an attacker. Investigators need to preserve evidence and document how it was collected. If evidence is altered without a record, it becomes harder to trust the conclusions.

A chain of custody records who handled evidence, when they handled it, and what happened to it. Time zones and clock differences matter too, because a timeline assembled from systems with mismatched clocks can be misleading.

## Thinking like an investigator

I should distinguish observations from conclusions. For example, a log entry may show a login from an IP address, but it does not by itself prove who was physically using the device. I need corroborating information and should record uncertainty rather than force every clue into one story.

In a lab, the safest workflow is to follow the provided evidence and use the tools the task asks for. I should not invent timestamps, hashes, or findings that are not present in the artefacts.

## What I took from this room

DFIR combines careful technical work with disciplined record keeping. The goal is to contain the incident while preserving enough evidence to understand how it happened. A good investigation explains what the evidence supports, what remains uncertain, and what should be done next.