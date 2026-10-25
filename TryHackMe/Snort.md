# Snort: TryHackMe Walkthrough

Room: https://tryhackme.com/room/snort

Snort is a network intrusion detection and prevention system. It can inspect network traffic and compare packets or flows against rules. In IDS mode it alerts on matches; in IPS-style deployment it may block traffic, depending on how it is configured and placed in the network.

## 1. Understand the rule structure

A typical Snort rule contains an action, protocol, source address and port, direction operator, destination address and port, and a set of options. The options can include a message, rule identifier, content match, and other conditions.

A simplified example for learning the structure is:

    alert tcp any any -> 192.0.2.10 80 (msg:"Example HTTP alert"; sid:1000001; rev:1;)

This is an illustrative rule, not a rule intended for a real production network. The documentation for the installed Snort version should be used because syntax and capabilities differ between versions.

## 2. Understand the traffic and rule

A rule should match the traffic it is designed to detect. The protocol, address direction, port, and content conditions all matter. A rule that is too broad may produce many false positives; a rule that is too narrow may miss variations of the behaviour.

Use the packet capture or traffic supplied by the room. Identify the relevant source and destination, protocol, and payload. Then compare those details with the rule's conditions. Do not infer that a rule triggered unless the tool output confirms the alert.

## 3. Run Snort in the lab

Use the command and configuration provided by the active room. Common options can select a configuration file, read packets from a capture, or choose an interface for live traffic. The exact command depends on the Snort version and the exercise.

Before running a rule set, validate the configuration using the installed version's supported test option. If the parser reports an error, check rule syntax, parentheses, quotation marks, the SID, and whether the selected version supports the option.

## 4. Interpret alerts

An alert should identify the rule that matched and provide enough context to investigate the event. Read the message and SID, then correlate the alert with the packet or flow. A rule match is a signal, not proof of compromise. Benign software can generate traffic that resembles a malicious pattern.

For a useful investigation, record the timestamp, source and destination, rule ID, packet context, and why the activity is considered suspicious. Keep the original capture when permitted so another analyst can reproduce the conclusion.

## 5. Tune and maintain rules

- Use unique local SIDs according to the site's rule-management policy.
- Keep rule messages meaningful and specific.
- Test rules against both malicious and benign samples.
- Review false positives and missed detections.
- Keep Snort and rule sets updated.
- Document the purpose and owner of custom rules.
- Avoid deploying untested blocking rules that could disrupt legitimate traffic.

## 6. Working through the room

Follow the task's provided PCAP, configuration, or command. Identify which rule or packet feature the question refers to and read the actual output. If the task asks for an alert message or SID, use the result from the live environment instead of copying an example from this note.

## What I took from this room

Snort turns packet-level observations into repeatable detections. The quality of the result depends on understanding the traffic, writing accurate rules, validating the configuration, and investigating alerts in context.