# Networking Concepts

{% embed url="https://tryhackme.com/room/networkingconcepts" %}

Core networking models, OSI Layer 1 through Layer 7 duties, TCP/IP mappings, IPv4 subnetting rules, TCP vs UDP transmission characteristics, and packet encapsulation.

---

## 1. The OSI Model

- **Layer 7 (Application):** End-user network interfaces (HTTP, DNS, SSH).
- **Layer 6 (Presentation):** Data encoding, encryption, and translation (SSL/TLS, ASCII).
- **Layer 5 (Session):** Session establishment and maintenance.
- **Layer 4 (Transport):** Process-to-process communication connecting applications (TCP, UDP).
- **Layer 3 (Network):** Logical addressing and routing packets between distinct networks (IP).
- **Layer 2 (Data Link):** Physical frame delivery between hosts on the same network segment (Ethernet, Wi-Fi MAC).
- **Layer 1 (Physical):** Raw bitstream transmission across physical media.

---

## 2. TCP/IP Model Mapping

The 4-layer TCP/IP suite consolidates OSI Layers 5, 6, and 7 into a single **Application Layer**.

---

## 3. IPv4 Addressing & Protocols

- **Private IP Ranges (RFC 1918):**
  - `10.0.0.0/8`
  - `172.16.0.0/12`
  - `192.168.0.0/16`
  *(e.g., `49.69.147.197` is a public routable address).*
- **Valid Octets:** Octet values must fall strictly between `0` and `255` (e.g. `192.168.305.19` is invalid).
- **Transport Differences:**
  - **TCP:** Connection-oriented, reliable, requires a 3-way handshake (SYN, SYN-ACK, ACK).
  - **UDP:** Connectionless, fast, best-effort.
  - **Total Ports:** Approximately 65,535 (`~65 thousand`).

---

## 4. Encapsulation & Protocol Data Units (PDUs)

```
Application Data -> Segment (TCP) / Datagram (UDP) -> Packet (IP) -> Frame (Ethernet/Wi-Fi) -> Bits
```

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | Layer responsible for connecting one application to another | `Layer 4` |
| **Task 2** | Layer responsible for routing packets to proper network | `Layer 3` |
| **Task 2** | OSI layer responsible for encoding application data | `Layer 6` |
| **Task 2** | Layer responsible for transferring data between hosts on same segment | `Layer 2` |
| **Task 3** | Layer to which HTTP belongs in TCP/IP model | `Application Layer` |
| **Task 3** | How many OSI layers does TCP/IP Application layer cover? | `3` |
| **Task 4** | Address that is not a private IP address | `49.69.147.197` |
| **Task 4** | Address that is not a valid IP address | `192.168.305.19` |
| **Task 5** | Protocol requiring a 3-way handshake | `TCP` |
| **Task 5** | Approximate number of port numbers (in thousands) | `65` |
| **Task 6** | PDU encapsulating an IP packet on Wi-Fi | `Frame` |
| **Task 6** | PDU encapsulating application data over UDP | `Datagram` |
| **Task 6** | PDU encapsulating application data over TCP | `Segment` |
