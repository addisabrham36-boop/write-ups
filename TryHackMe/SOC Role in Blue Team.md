# SOC Role in the Blue Team — TryHackMe

Room: https://tryhackme.com/room/socrole

A Security Operations Center helps an organisation monitor systems, investigate alerts, and respond to threats. The SOC is part of the blue team, which focuses on protecting systems and reducing the impact of attacks.

## What analysts do

Analysts review alerts from SIEM platforms, endpoint tools, network monitoring, and other security products. They gather context, look for related events, and decide whether an alert is benign, suspicious, or likely malicious. More complex incidents may be escalated to incident responders or specialist teams.

## The alert is only the start

A detection rule can produce false positives, and an attacker may also evade a rule. Analysts should inspect the underlying evidence and ask whether the activity fits the environment. A login from a new location, for example, may be suspicious, but VPN use or legitimate travel could explain it. More evidence is needed before drawing a conclusion.

## Communication and response

Good notes should include the alert, time range, affected assets, evidence reviewed, and reason for the decision. The SOC also needs a clear escalation process so serious events are not left sitting in a queue. After incidents, the team can improve detection rules and response procedures.

## What I took from this room

The SOC's value is not simply collecting alerts. It turns telemetry into investigation and action. Technical skills matter, but so do prioritisation, documentation, teamwork, and knowing when the evidence is not strong enough to support a conclusion.