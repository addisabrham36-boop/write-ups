# Snort — TryHackMe

Room: https://tryhackme.com/room/snort

Snort is a network intrusion detection and prevention system. It can inspect traffic and compare it with rules that describe suspicious patterns. Depending on configuration, it may alert on matching traffic or operate inline to block it.

## Rules and alerts

A Snort rule usually describes the traffic to inspect, the direction, and the condition that should trigger an action. Rule quality matters: an overly broad rule can create many false positives, while a narrow rule may miss variations of the same behaviour.

A detection alert is evidence that traffic matched a rule, not automatic proof that a system was compromised. Analysts should review the packet context, source and destination, time, and related events.

## Running a local rule check

In a controlled environment, Snort options depend on the installed version and configuration. I should check the local help and the room's instructions before using a command, because Snort 2 and Snort 3 have differences in syntax and configuration.

For a ruleset or packet capture provided by the lab, the safe workflow is to load the specified configuration, run the instructed analysis, and inspect the alert output. I should not invent a command or claim an alert occurred without seeing the actual result.

## What I took from this room

Snort turns network traffic into rule-based detections. Effective use depends on well-written rules, correct configuration, and analysis of the surrounding evidence. The tool can highlight suspicious activity, but a human still needs to decide what the alert means.