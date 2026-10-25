# Nmap: Live Host Discovery — Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/nmap01

## Room Overview & Information

**Category:** Network reconnaissance  
**Difficulty:** Beginner  
**Topics:** Host discovery, ARP, ICMP probes, TCP SYN/ACK ping, UDP probes, reverse DNS, and subnet calculations.

Nmap host discovery identifies addresses that appear online before a port scan. Different probes suit different network environments. A host may ignore ICMP but respond to TCP; a system on the same Ethernet segment may respond to ARP even when IP-layer probes are filtered.

This room includes interactive packet visualisations. Read the simulation carefully when a question references the first versus second ARP request.

## Phase 1: Reconnaissance & Target Enumeration

### Task 1: Introduction

Start the room's interactive lab and follow the packet visualisation. The activity uses simulated computers and a router. Answer from the current simulation rather than assuming every network behaves identically.

### Task 2: Enumerating Targets

**Question:** “What is the first IP address Nmap would scan if you provided 10.10.12.13/29 as your target?”

**Answer:** **10.10.12.8**.

A /29 prefix contains eight IPv4 addresses. The subnet containing 10.10.12.13/29 begins at 10.10.12.8. The network address is 10.10.12.8 and the broadcast address is 10.10.12.15.

**Question:** “How many IP addresses will Nmap scan if you provide the following range 10.10.0-255.101-125?”

**Answer:** **6400**.

The third octet spans 256 values (0 through 255). The fourth spans 25 values (101 through 125 inclusive). Therefore 256 × 25 = 6400 addresses.

## Phase 2: ARP and ICMP Discovery Analysis

### Task 3: Discovering Hosts

ARP maps an IPv4 address to a MAC address on the local network. If a host needs to send data to another local IPv4 address and the mapping is not cached, it broadcasts an ARP Request. The destination may reply with an ARP Response. ARP broadcasts are limited to the local Layer 2 broadcast domain and are not routed across the Internet.

**Question:** “How many devices can see the ARP Request?”

**Answer:** **4**.

The simulation shows the broadcast request being visible to four devices on the local segment.

**Question:** “Did computer6 receive the ARP Request? (Y/N)”

**Answer:** **N**.

In the first scenario, the visualised packet path shows that computer6 does not receive that request.

**Question:** “How many devices can see the ARP Request?”

**Answer:** **4**.

This question repeats in a later interactive step. Use the packet visualisation for that scenario as well.

**Question:** “Did computer6 reply to the ARP Request? (Y/N)”

**Answer:** **Y**.

The second ARP scenario shows computer6 replying to the request. It is a different request from the earlier receive question.

**Question:** “What is the type of packet that computer1 sent before the ping?”

**Answer:** **ARP Request**.

Computer1 must resolve the local MAC address before it can send an Ethernet frame to the destination.

**Question:** “What is the type of packet that computer1 received before being able to send the ping?”

**Answer:** **ARP Response**.

The response supplies the IP-to-MAC mapping needed for local frame delivery.

**Question:** “How many computers responded to the ping request?”

**Answer:** **1**.

Only one computer responds to the ping in the simulation. This differs from the number of devices that can see the broadcast ARP request.

**Question:** “What is the name of the first device that responded to the first ARP Request?”

**Answer:** **router**.

**Question:** “What is the name of the first device that responded to the second ARP Request?”

**Answer:** **computer5**.

**Question:** “Send another Ping Request. Did it require new ARP Requests? (Y/N)”

**Answer:** **N**.

The host has cached the IP-to-MAC mapping from the earlier exchange, so another ARP request is not required immediately.

**Question:** “How many devices are you able to discover using ARP requests?”

**Answer:** **3**.

This is the result shown by the room's ARP discovery simulation.

## Phase 3: Nmap Host-Discovery Techniques

### Task 4: Nmap Host Discovery Controls

Nmap provides different discovery probes. These options help determine whether a host responds; they do not mean that every port is being scanned.

- **-PE:** ICMP Echo Request.
- **-PP:** ICMP Timestamp Request.
- **-PM:** ICMP Address Mask Request.
- **-sn:** host discovery only, skipping the port scan.
- **-R:** force reverse DNS resolution for target addresses.
- **-PS:** TCP SYN discovery probe, optionally followed by port numbers.
- **-PA:** TCP ACK discovery probe, optionally followed by port numbers.
- **-PU:** UDP discovery probe, optionally followed by port numbers.

**Question:** “What is the option required to tell Nmap to use ICMP Timestamp to discover live hosts?”

**Answer:** **-PP**.

**Question:** “What is the option required to tell Nmap to use ICMP Address Mask to discover live hosts?”

**Answer:** **-PM**.

**Question:** “What is the option required to tell Nmap to use ICMP Echo to discover live hosts?”

**Answer:** **-PE**.

Example using ICMP Echo to discover hosts in an authorised lab subnet:

~~~bash
nmap -sn -PE -oN icmp-discovery.txt TARGET_SUBNET
~~~

- **-sn** skips the port scan.
- **-PE** sends ICMP Echo discovery probes.
- **-oN** saves readable output.
- **TARGET_SUBNET** is the exact range supplied by the room.

### Task 5: TCP and UDP Host Discovery

TCP SYN probes send a SYN packet to a selected port and observe the response. TCP ACK probes send an ACK packet and observe the response. Firewalls may treat these differently.

**Question:** “Which TCP ping scan does not require a privileged account?”

**Answer:** **TCP SYN Ping**.

**Question:** “Which TCP ping scan requires a privileged account?”

**Answer:** **TCP ACK Ping**.

**Question:** “What option do you need to add to Nmap to run a TCP SYN ping scan on the telnet port?”

**Answer:** **-PS23**.

Example:

~~~bash
nmap -sn -PS23 -oN tcp-syn-discovery.txt TARGET_SUBNET
~~~

The **-PS23** option uses TCP SYN probes directed at TCP port 23, conventionally used for Telnet. Use only the room's authorised address range.

UDP discovery can be used where appropriate. Nmap's **-PU** option sends UDP probes, optionally to selected ports. A lack of UDP response is ambiguous because a service may ignore an unexpected datagram.

### Task 6: Reverse DNS Lookup

Reverse DNS looks for PTR records that map IP addresses to names. Names can be missing, generic, or stale.

**Question:** “We want Nmap to issue a reverse DNS lookup for all the possibles hosts on a subnet, hoping to get some insights from the names. What option should we add?”

**Answer:** **-R**.

Example:

~~~bash
nmap -sn -R -oN reverse-dns.txt TARGET_SUBNET
~~~

**-R** forces reverse DNS lookups for target addresses. This can make scans slower and does not guarantee that every IP has a PTR record.

## Phase 4: Room Questions & Answer Summary

| Task | Question | Answer |
|---|---|---|
| 2 | First IP for 10.10.12.13/29 | 10.10.12.8 |
| 2 | Number of addresses in 10.10.0-255.101-125 | 6400 |
| 3 | Devices able to see the ARP Request | 4 |
| 3 | Did computer6 receive the first ARP Request? | N |
| 3 | Did computer6 reply to the second ARP Request? | Y |
| 3 | Packet sent before ping | ARP Request |
| 3 | Packet received before ping | ARP Response |
| 3 | Computers responding to ping | 1 |
| 3 | First responder to the first ARP Request | router |
| 3 | First responder to the second ARP Request | computer5 |
| 3 | Did another ping require new ARP requests? | N |
| 3 | Devices discoverable through ARP in the simulation | 3 |
| 4 | ICMP Timestamp option | -PP |
| 4 | ICMP Address Mask option | -PM |
| 4 | ICMP Echo option | -PE |
| 5 | TCP ping not requiring privileged account | TCP SYN Ping |
| 5 | TCP ping requiring privileged account | TCP ACK Ping |
| 5 | TCP SYN ping to Telnet port | -PS23 |
| 6 | Force reverse DNS resolution | -R |

## Phase 5: Troubleshooting & Defensive Takeaways

- A host that does not answer ICMP can still be online.
- ARP discovery works on the local Ethernet segment and is not routed across the Internet.
- Cached ARP entries can remove the need for another exchange.
- TCP SYN and ACK probes behave differently and may require different privileges.
- Reverse DNS may return no name or an outdated name.
- Host-discovery output does not prove that a port is open or a service is vulnerable.
- Scan only the target range authorised by the room and save output for reproducibility.

Host discovery is about choosing a probe suited to the network. Interpret responses in context, preserve the command and output, and do not treat silence as proof that a host is offline.