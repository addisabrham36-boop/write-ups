# Advent of Cyber 2023 - Day 18: Incident Response

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Linux incident response and eradication: Hunting persistent malware, process trees, and rogue **systemd services**.

## Walkthrough
1. Check active resource-intensive processes:
   ```bash
   top
   ps aux | grep -i crypt
   ```
2. Killing the rogue process (`kill -9 PID`) results in immediate respawning under a new PID.
3. Inspect systemd services:
   ```bash
   systemctl list-unit-files --state=enabled
   ls -la /etc/systemd/system/
   ```
4. A rogue unit file named `a-unkillable.service` located in `/etc/systemd/system` continuously respawns the binary.
5. Disable and remove the service:
   ```bash
   sudo systemctl stop a-unkillable.service
   sudo systemctl disable a-unkillable.service
   sudo rm /etc/systemd/system/a-unkillable.service
   sudo systemctl daemon-reload
   ```

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Name of service that respawns process after killing it? | `a-unkillable.service` |
| Path from where process and service were running? | `/etc/systemd/system` |
