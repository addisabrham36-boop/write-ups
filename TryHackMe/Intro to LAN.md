# Intro to LAN

Official room: https://tryhackme.com/room/introtolan

## Room Overview & Information

**Category:** Networking fundamentals  
**Difficulty:** Beginner  
**Topics:** LAN topologies, switches and routers, IPv4 subnetting, ARP, and DHCP.

This is an interactive networking fundamentals room. There is no boot-to-root target, so exploitation and privilege escalation are not applicable. Complete each interactive diagram through the room's View Site control and use the values shown by the live exercise.

## Phase 1: Reconnaissance & Network Concept Enumeration

### Task 1: LAN Topologies

A Local Area Network (LAN) connects devices within a limited area such as a home, classroom, lab, or office. A topology describes how devices are physically or logically connected.

- **Star topology:** Each endpoint connects to a central switch or hub. It is easy to add devices and troubleshoot individual links, but the central device is a potential single point of failure.
- **Bus topology:** Devices share a backbone cable. It can use less cabling but faults on the backbone may disrupt the network and troubleshooting becomes harder as the network grows.
- **Ring topology:** Devices form a loop and data travels around it. A simple ring can be disrupted by a broken link unless redundancy is designed into it.

A switch connects devices within a LAN and forwards Ethernet frames using MAC addresses. A router connects different IP networks and forwards packets using routing information.

### Task 1 questions

**Question: “What does LAN stand for?”**

**Answer:** Local Area Network.

**Question: “What is the verb given to the job that Routers perform?”**

**Answer:** Routing.

**Question: “What device centrally connects multiple devices and transmits data to the correct location?”**

**Answer:** Switch.

**Question: “What topology is cost-efficient to set up?”**

**Answer:** Bus Topology.

**Question: “What topology is expensive to set up and maintain?”**

**Answer:** Star Topology.

### Interactive topology flag

Open the topology diagram with View Site and complete the activity about the topology and its failure. Follow the simulator's feedback to obtain the flag. The exact value is generated or displayed by the live room and is therefore redacted as **THM{hidden}** here.

## Phase 2: IP Addressing, Subnetting & Local Resolution

### Task 2: Subnetting

Subnetting divides a larger IP network into smaller logical networks. It helps manage address space, organise devices, and separate systems by role or trust level.

An IPv4 address contains 32 bits, commonly written as four decimal octets. Each octet ranges from 0 to 255. A subnet mask or CIDR prefix identifies the network portion.

For example, **192.168.1.100/24** is an illustrative host in the **192.168.1.0/24** network. The prefix /24 means the first 24 bits identify the network; this example is not a value to copy into a live task.

Important address roles:
- **Network address:** identifies the network.
- **Host address:** identifies a device within the network.
- **Default gateway:** the router a host normally uses to reach a different network.

### Task 2 questions

**Question: “What is the technical term for dividing a network into smaller pieces?”**

**Answer:** Subnetting.

**Question: “How many bits are in a subnet mask?”**

**Answer:** 32 for IPv4.

**Question: “What is the range of a section (octet) of a subnet mask?”**

**Answer:** 0–255.

**Question: “What address identifies the start of a network?”**

**Answer:** Network Address.

**Question: “What address identifies devices within a network?”**

**Answer:** Host Address.

**Question: “What is the device responsible for sending data to another network called?”**

**Answer:** Default Gateway.

### Task 3: ARP Protocol

Address Resolution Protocol (ARP) allows an IPv4 host to discover the MAC address associated with an IPv4 address on the local network.

A typical exchange works like this:
1. Host A needs to send data to a local IPv4 address.
2. If no cached mapping exists, it broadcasts an ARP Request.
3. The target replies with an ARP Reply containing its MAC address.
4. Host A stores the mapping and uses it to deliver Ethernet frames.

ARP is used on the local link. It does not replace IP routing and does not carry the same Ethernet frame unchanged across the Internet.

### Task 3 questions

**Question: “What does ARP stand for?”**

**Answer:** Address Resolution Protocol.

**Question: “What category of ARP packet asks a device whether it has a specific IP address?”**

**Answer:** Request.

**Question: “What address is used as a physical identifier for a device?”**

**Answer:** MAC Address.

**Question: “What address is used as a logical identifier for a device?”**

**Answer:** IP Address.

## Phase 3: DHCP Configuration & Verification

### Task 4: DHCP Protocol

Dynamic Host Configuration Protocol (DHCP) automatically supplies network settings such as IP address, subnet mask, default gateway, DNS resolver, and lease duration.

The four-step exchange is commonly called DORA:
1. **Discover:** the client searches for a DHCP server.
2. **Offer:** a server offers configuration.
3. **Request:** the client requests the offered address.
4. **ACK:** the server acknowledges the lease.

A rogue DHCP server can supply malicious gateway or DNS settings, so managed networks should restrict which devices may provide DHCP.

### Task 4 questions

**Question: “What type of DHCP packet does a device use to retrieve an IP address?”**

**Answer:** DHCP Discover.

**Question: “What type of DHCP packet does a device send once offered an IP address?”**

**Answer:** DHCP Request.

**Question: “What is the last DHCP packet sent from a server to a device?”**

**Answer:** DHCP ACK.

### Troubleshooting commands

On a Linux system, use:

```bash
ip addr
ip route
ip neigh
```

- **ip addr** displays interface addresses.
- **ip route** displays routes and the default gateway.
- **ip neigh** displays cached neighbour mappings, including IP-to-MAC information.

The commands show the current machine's configuration. A successful ping does not prove every service is available because firewalls can filter ICMP while permitting other protocols.

## Phase 4: Room Questions & Flag Walkthrough

| Task | Question focus | Answer |
|---|---|---|
| Task 1 | LAN abbreviation | Local Area Network |
| Task 1 | Router's job | Routing |
| Task 1 | Central LAN device | Switch |
| Task 1 | Cost-efficient topology | Bus Topology |
| Task 1 | More expensive topology | Star Topology |
| Task 2 | Dividing a network | Subnetting |
| Task 2 | IPv4 subnet mask size | 32 bits |
| Task 2 | Octet range | 0–255 |
| Task 2 | Network start | Network Address |
| Task 2 | Device identifier within network | Host Address |
| Task 2 | Router used to reach other networks | Default Gateway |
| Task 3 | ARP abbreviation | Address Resolution Protocol |
| Task 3 | ARP query type | Request |
| Task 3 | Physical address | MAC Address |
| Task 3 | Logical address | IP Address |
| Task 4 | DHCP first packet | DHCP Discover |
| Task 4 | Client requests offered address | DHCP Request |
| Task 4 | Server's final DHCP packet | DHCP ACK |
| Topology interactive exercise | Flag | Read from the active site; redacted as THM{hidden} |
