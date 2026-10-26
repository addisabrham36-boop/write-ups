# Nmap Live Host Discovery

{% embed url="https://tryhackme.com/room/nmap01" %}

How Nmap discovers which hosts are alive before port scanning — ARP requests, ICMP echo, TCP/UDP probes. Includes an interactive network simulator.

---

## Why Host Discovery?

Nmap doesn't blindly scan all IPs. It first checks which hosts are up to avoid wasting time. The method depends on your network context.

---

## Techniques

| Technique | Command | Best for |
|---|---|---|
| ARP scan | `nmap -PR -sn RANGE` | Same subnet — most reliable |
| ICMP echo | `nmap -PE -sn TARGET` | Cross-subnet ping sweep |
| ICMP timestamp | `nmap -PP -sn TARGET` | Bypass ICMP echo blocks |
| TCP SYN ping | `nmap -PS80,443 -sn TARGET` | Firewall-heavy environments |
| TCP ACK ping | `nmap -PA80 -sn TARGET` | Firewalls that pass ACK |
| UDP ping | `nmap -PU53 -sn TARGET` | DNS/UDP-only hosts |

`-sn` = skip port scan, just do host discovery.

---

## ARP — How it Works

Before sending an ICMP ping, the OS needs a MAC address. If the destination is on the same subnet, it sends an **ARP Request** broadcast. Only the target replies with an **ARP Response**.

```bash
sudo nmap -PR -sn 192.168.1.0/24
```

---

## Interactive Simulator — Answers

| Question | Answer |
|---|---|
| Packet type sent by computer1 before the ping | `ARP Request` |
| Packet type computer1 received before pinging | `ARP Response` |
| How many computers responded to the ping? | `1` |
| First device to respond to first ARP Request | `router` |
| First device to respond to second ARP Request | `computer5` |
| Did the second ping require new ARP Requests? | `N` |

---

## Useful Flags

```bash
nmap -sn TARGET_IP          # Host discovery only (no port scan)
nmap -v -sn RANGE           # Verbose host discovery
nmap --reason TARGET_IP     # Show why host is up
```

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Packet sent before ping | `ARP Request` |
| Packet received before ping | `ARP Response` |
| Computers responding to ping | `1` |
| First device to respond to ARP Request 1 | `router` |
| First device to respond to ARP Request 2 | `computer5` |
| Did second ping need new ARP? | `N` |
