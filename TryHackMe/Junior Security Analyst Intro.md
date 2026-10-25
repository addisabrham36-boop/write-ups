# Junior Security Analyst Intro — TryHackMe

Room: https://tryhackme.com/room/jrsecanalystintrouxo

A junior security analyst often works in a Security Operations Center, monitoring alerts and helping investigate suspicious activity. The job is not just clicking an alert and declaring an attack. The analyst needs to collect context, assess evidence, and decide what should happen next.

## Triage an alert

I would start by reading the alert description and the detection rule that created it. Then I would identify the affected host, user, source IP, timestamp, and relevant process or event. I would search nearby logs to see what happened before and after the alert.

For example, several failed logins followed by a successful login may be worth investigating. But I need to check whether the source is expected, whether the user was travelling or using a VPN, and whether other activity supports the suspicion.

## Escalation and notes

If evidence suggests a real incident or the activity exceeds my authority, I should escalate it using the organisation's procedure. My notes should explain what I checked, what I found, and why I reached the current conclusion. A teammate should be able to follow the investigation without guessing what I meant.

## Skills that help

Networking, operating systems, log analysis, basic scripting, and clear written communication all matter. I also need to know when I do not have enough evidence. Being honest about uncertainty is better than making a confident but unsupported claim.

## What I took from this room

The analyst's job is to turn alerts into decisions. That requires understanding the environment, validating evidence, and documenting the investigation. Good triage reduces noise while making sure serious activity is not ignored.