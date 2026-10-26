# Advent of Cyber 2024 - Day 24: MQTT

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

You can't hurt SOC-mas!

## Background

Message Queuing Telemetry Transport (MQTT) is an ISO standard publish-subscribe messaging protocol designed for resource-constrained IoT devices and low-bandwidth networks.

MQTT relies on an intermediary broker listening on port 1883 (plain) or 8883 (TLS). Clients publish messages to topics and subscribe to topic hierarchies. Topics use `/` delimiters and wildcards (`#` for multi-level, `+` for single-level). Unauthenticated brokers allow adversaries to snoop on telemetry or publish unauthorized commands.

## Walkthrough

**Step 1 — Subscribe to All MQTT Topics**

Connect to the broker and listen on all topics using `#`:
```bash
mosquitto_sub -h TARGET_IP -t "#" -v
```
Observe incoming messages on `/city/lights/status` and `/security/alarm`.

**Step 2 — Publish Override Command**

Send a command message to restore the municipal lighting grid:
```bash
mosquitto_pub -h TARGET_IP -t "/city/lights/cmd" -m "ON"
```

**Step 3 — Capture Final Event Flag**

The broker broadcasts status confirmation containing the final Advent of Cyber flag: `THM{Ligh75on-day54ved}`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What default TCP port is used by unencrypted MQTT brokers? | `1883` |
| What wildcard character subscribes to all topics in MQTT? | `#` |
| What is the final flag retrieved after saving SOC-mas? | `THM{Ligh75on-day54ved}` |
