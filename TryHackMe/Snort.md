# Snort

{% embed url="https://tryhackme.com/room/snort" %}

A comprehensive hands-on room covering Snort, the open-source Network Intrusion Detection and Prevention System (NIDS/NIPS). This guide covers rule syntax, PCAP analysis, operation modes, and alert management.

---

## 1. Introduction to IDS/IPS

- **NIDS (Network Intrusion Detection System):** Monitors and detects threats across an entire local network.
- **NIPS (Network Intrusion Prevention System):** Inspects network traffic inline and actively drops malicious packets.
- **HIDS (Host-based Intrusion Detection System):** Monitors system logs, file integrity, and process activity on a local machine.
- **HIPS (Host-based Intrusion Prevention System):** Actively blocks malicious operations directly on the local machine.

---

## 2. Operation Modes & Commands

Test configuration:
```bash
snort -c /etc/snort/snort.conf -T
```

Sniff live traffic:
```bash
snort -v -i eth0
```

Read and analyze PCAP files:
```bash
snort -r traffic.pcap -c /etc/snort/rules/local.rules -A console
```

---

## 3. Snort Rule Anatomy

Rules consist of a **Rule Header** and **Rule Options**:

```text
[action] [protocol] [src_ip] [src_port] [direction] [dst_ip] [dst_port] (options)
```

Example rule checking for HTTP traffic with a custom SID:
```text
alert tcp any any -> any 80 (msg:"HTTP Connection Detected"; sid:1000001; rev:1;)
```

### Essential Rule Options:
- `msg:"..."`: Text displayed in the alert log.
- `sid`: Snort Rule ID (custom rules start at `1000001`+).
- `rev`: Rule revision number.
- `flags`: Inspect TCP flags (e.g. `flags:S;` for SYN packets, `flags:PA;` for PSH-ACK).
- `content:"..."`: Payload string matching.

---

## 4. Interactive Exercises

In the `Task-Exercises` folder on the lab desktop:
```bash
cd Task-Exercises
./.easy.sh
```
Output:
`Too Easy!`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | Output from running `./.easy.sh` | `Too Easy!` |
| **Task 3** | Which IDS/IPS stops threats on a local machine? | `HIPS` |
| **Task 3** | Which IDS/IPS detects threats on a local network? | `NIDS` |
| **Task 3** | Which IDS/IPS detects threats on a local machine? | `HIDS` |
| **Task 4** | Parameter used to test the configuration file | `-T` |
| **Task 4** | Parameter used to read a PCAP file | `-r` |
| **Task 4** | Console alert output mode switch | `-A console` |
| **Task 9** | Default file for writing custom user rules | `local.rules` |
