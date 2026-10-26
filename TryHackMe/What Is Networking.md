# What Is Networking?

{% embed url="https://tryhackme.com/room/whatisnetworking" %}

An introduction to computer networking principles: network definitions, the origin of the World Wide Web, IP addresses, MAC addresses, address spoofing, and the ICMP ping utility.

---

## 1. What is Networking?

A **Network** is a collection of two or more computing devices connected together to share data, resources, and communication.

---

## 2. The Internet vs The World Wide Web

- **The Internet:** The global physical infrastructure of interconnected networks.
- **The World Wide Web (WWW):** The information-sharing system built on top of the internet using hyperlinks and web pages, invented by **Tim Berners-Lee**.

---

## 3. Network Identifiers (IP & MAC)

- **IP (Internet Protocol):** Logical address assigned to a device on a network (IPv4 consists of **4 sections** called **octets**, each ranging 0-255).
- **MAC (Media Access Control):** Physical hardware identifier burned into the Network Interface Card (NIC).
- **Interactive Lab:** Spoofing the network interface MAC address to access a restricted portal yields:
  `flag{hidden}`

---

## 4. Ping & ICMP

The `ping` utility tests reachability between hosts using **ICMP** (Internet Control Message Protocol).
- Syntax: `ping 10.10.10.10`
- Pinging `8.8.8.8` in the simulation completes the test and returns:
  `flag{hidden}`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 1** | Key term for devices that are connected together | `Network` |
| **Task 2** | Who invented the World Wide Web? | `Tim Berners-Lee` |
| **Task 3** | What does IP stand for? | `Internet Protocol` |
| **Task 3** | What is each section of an IP address called? | `Octet` |
| **Task 3** | How many sections (digits) does an IP address have? | `4` |
| **Task 3** | What does MAC stand for? | `Media Access Control` |
| **Task 3** | Flag from spoofing MAC address in interactive lab | `flag{hidden}` |
| **Task 4** | What protocol does ping use? | `ICMP` |
| **Task 4** | What is the syntax to ping 10.10.10.10? | `ping 10.10.10.10` |
| **Task 4** | Flag received when you ping 8.8.8.8 | `flag{hidden}` |
