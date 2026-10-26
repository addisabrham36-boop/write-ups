# Intro to LAN

{% embed url="https://tryhackme.com/room/introtolan" %}

Explore Local Area Network (LAN) topologies, subnetting concepts, and the operation of foundational link and network layer protocols including ARP and DHCP.

---

## 1. LAN Topologies

A **Local Area Network (LAN)** connects devices within a limited physical geographical area.
- **Routing:** The action performed by routers to move packets across distinct networks.
- **Switch:** Centrally connects multiple devices on the same local network segment and forwards frames based on MAC address.
- **Bus Topology:** Cost-efficient to set up; single cable backbone.
- **Star Topology:** Most common in modern networks; devices connect to a central switch. More expensive to set up and maintain due to required cabling and switching hardware.
- **Interactive Lab:** Completing the topology simulation reveals the flag:
  `THM{TOPOLOGY_FLAWS}`

---

## 2. Subnetting Primer

- **Subnetting:** The process of dividing a large physical network into smaller, logical sub-networks.
- **Subnet Mask:** A 32-bit value dividing the network portion from the host portion.
- **Octet Range:** Each 8-bit octet ranges from `0-255`.
- **Network Address:** The first address in a subnet, identifying the network itself.
- **Host Address:** Specific assignable IP identifying a device within the subnet.
- **Default Gateway:** The router interface responsible for forwarding packets destined outside the local subnet.

---

## 3. ARP (Address Resolution Protocol)

ARP resolves known logical **IP Addresses** to physical hardware **MAC Addresses** on a local network segment.
- **ARP Request:** Sent as a broadcast asking "Who has IP X.X.X.X? Tell Y.Y.Y.Y".
- **ARP Reply/Response:** Unicast message sent back with the requested MAC address.

---

## 4. DHCP (Dynamic Host Configuration Protocol)

DHCP automatically assigns IP addresses, subnet masks, gateways, and DNS servers to joining devices through the 4-step **DORA** exchange:
1. **Discover:** Broadcast by client to locate available DHCP servers.
2. **Offer:** Unicast/broadcast from server offering an available IP.
3. **Request:** Broadcast by client requesting the offered IP.
4. **ACK (Acknowledge):** Final confirmation from server reserving the lease.

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 1** | What does LAN stand for? | `Local Area Network` |
| **Task 1** | Verb for the job routers perform | `Routing` |
| **Task 1** | Device used to centrally connect multiple devices on local network | `Switch` |
| **Task 1** | Topology cost-efficient to set up | `Bus Topology` |
| **Task 1** | Topology expensive to set up and maintain | `Star Topology` |
| **Task 1** | Flag given at the end of the topology lab | `THM{TOPOLOGY_FLAWS}` |
| **Task 2** | Technical term for dividing a network into smaller pieces | `Subnetting` |
| **Task 2** | How many bits are in a subnet mask? | `32` |
| **Task 2** | Range of an octet section in a subnet mask | `0-255` |
| **Task 2** | Address used to identify the start of a network | `Network Address` |
| **Task 2** | Address used to identify devices within a network | `Host Address` |
| **Task 2** | Device responsible for sending data to another network | `Default Gateway` |
| **Task 3** | What does ARP stand for? | `Address Resolution Protocol` |
| **Task 3** | Category of ARP packet asking if a device has an IP | `Request` |
| **Task 3** | Physical identifier for a device on a network | `MAC Address` |
| **Task 3** | Logical identifier for a device on a network | `IP Address` |
| **Task 4** | DHCP packet sent by device to retrieve an IP | `DHCP Discover` |
| **Task 4** | DHCP packet sent once IP is offered | `DHCP Request` |
| **Task 4** | Final DHCP packet sent from server confirming lease | `DHCP ACK` |
