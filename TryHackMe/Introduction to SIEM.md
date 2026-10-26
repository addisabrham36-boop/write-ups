# Introduction to SIEM

{% embed url="https://tryhackme.com/room/introtosiem" %}

What a SIEM is, how it ingests logs, generates alerts, and how to triage them. Ends with an interactive lab where a crypto miner triggers a real alert.

---

## What is a SIEM?

SIEM = **Security Information and Event Management system**. It collects logs from across an infrastructure, correlates events, and fires alerts on suspicious patterns.

Two types of data:
- **Host-centric** — activity on individual machines (file access, registry changes, process launches)
- **Network-centric** — traffic between hosts (VPN sessions, firewall logs, DNS queries)

---

## Log Sources

| Source | Type | Example path |
|---|---|---|
| Windows Event Logs | Host-centric | Event Viewer |
| Linux HTTP logs | Host-centric | `/var/log/httpd` |
| VPN logs | Network-centric | Firewall/gateway |
| Firewall logs | Network-centric | Perimeter device |

---

## Alerting

- **True-Positive** — alert correctly identifies real malicious activity
- **False-Positive (False alarm)** — alert fires but it's benign — needs tuning
- **Event ID 104** — Windows event generated when event logs are cleared/removed

---

## Lab — Catching the Crypto Miner

Click **Start Suspicious Activity** in the room's interactive lab. A SIEM alert fires. Investigate it:

- **Process that caused the alert:** `cudominer.exe`
- **Alert type:** `True-Positive` — it really is a miner

The flag is revealed in the lab interface once you correctly classify and dismiss the alert.

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What does SIEM stand for? | `Security Information and Event Management system` |
| Is registry activity host-centric or network-centric? | `host-centric` |
| Is VPN activity host-centric or network-centric? | `network-centric` |
| Where are HTTP logs stored on Linux? | `/var/log/httpd` |
| Which Event ID is generated when event logs are removed? | `104` |
| What type of alert may require tuning? | `False alarm` |
| Which process caused the alert in the lab? | `cudominer.exe` |
| Is the lab alert a True-Positive or False-Positive? | `True-Positive` |
