# Advent of Cyber 2022 - Day 21: IoT & MQTT

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Internet of Things (IoT) communication with the **MQTT** protocol (Message Queuing Telemetry Transport) and broker exploitation.

## Walkthrough
1. Port scan reveals port `1883/tcp` open (MQTT broker).
2. Subscribe to all topics using `mosquitto_sub`:
   ```bash
   mosquitto_sub -h TARGET_IP -t "#" -v
   ```
3. Topics reveal camera device feeds and telemetry messages.
4. Publish a command override to the camera control topic:
   ```bash
   mosquitto_pub -h TARGET_IP -t "device/camera/control" -m "disable_alarm"
   ```
5. The device acknowledges and displays the challenge flag.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| MQTT default port? | `1883` |
| IoT interaction flag | `THM{MQTT_MERRY_IOT}` |
