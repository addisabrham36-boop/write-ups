# Active Reconnaissance

{% embed url="https://tryhackme.com/room/activerecon" %}

Active recon means directly interacting with the target — ping, traceroute, Telnet, Netcat, browser DevTools. Unlike passive recon, packets actually reach the target.

---

## Web Browser as a Recon Tool

Open DevTools (F12) → Sources tab → inspect `script.js`. You can find how many questions the lab app contains.

**Answer: `8`**

---

## Ping

```bash
ping -s 128 TARGET_IP   # -s sets ICMP data size in bytes
ping -c 5 TARGET_IP     # -c sets count
```

| Question | Answer |
|---|---|
| Option to set ICMP data size | `-s` |
| ICMP header size | `8` bytes |
| Does MS Windows Firewall block ping by default? | `Y` |

---

## Traceroute

Maps the path packets take hop by hop using TTL increments.

```bash
traceroute TARGET_IP          # Linux — uses UDP by default
traceroute -i eth0 TARGET_IP  # -i to specify interface
```

| Question | Answer |
|---|---|
| Default protocol traceroute uses on Linux | `UDP` |
| Switch to specify an interface | `-i` |

---

## Telnet

Old protocol, unencrypted — everything in **cleartext**. Useful for banner grabbing:

```bash
telnet TARGET_IP 80
GET / HTTP/1.1
Host: TARGET_IP
```

| Question | Answer |
|---|---|
| Default Telnet port | `23` |
| Why is Telnet insecure? | Sends traffic in `cleartext` |

---

## Netcat

Swiss army knife for network connections:

```bash
nc TARGET_IP 80             # connect to port 80
echo "GET / HTTP/1.0" | nc TARGET_IP 80   # grab banner
nc -lvnp 4444               # listen mode
```

| Question | Answer |
|---|---|
| Option to specify port | `-p` |

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| How many questions does the lab browser app have? | `8` |
| Ping option to set ICMP data size | `-s` |
| ICMP header size in bytes | `8` |
| Does Windows Firewall block ping by default? | `Y` |
| Default protocol for Linux traceroute | `UDP` |
| Switch to specify interface in traceroute | `-i` |
| Default Telnet port | `23` |
| Why is Telnet insecure? | `cleartext` |
