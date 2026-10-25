# Nmap: Live Host Discovery — TryHackMe Walkthrough

Official room: https://tryhackme.com/room/nmap01

## Room Overview & Information

**Category:** Network reconnaissance  
**Difficulty:** Beginner  
**Focus:** Host discovery, ARP, ICMP probes, TCP SYN/ACK ping, UDP probes, reverse DNS, and subnet calculations.

Nmap host discovery determines which addresses appear to be active before a full port scan. Different probes work in different environments. A host may ignore ICMP but respond to TCP, and a device on the same Ethernet network may respond to ARP even when IP-layer probes are filtered.

## Task 1: Introduction

Start the room's interactive lab and follow the packet visualisation. The exercise uses simulated computers and a router. Answer from the current simulation rather than assuming that every network behaves identically.

## Task 2: Enumerating Targets

### Question: “What is the first IP address Nmap would scan if you provided 10.10.12.13/29 as your target?”

**Answer:** `10.10.12.8`.

A /29 prefix contains eight IPv4 addresses. The subnet containing 10.10.12.13/29 begins at 10.10.12.8, so that is the first address in the range. The network address is 10.10.12.8 and the broadcast address is 10.10.12.15.

### Question: “How many IP addresses will Nmap scan if you provide the following range 10.10.0-255.101-125?”

**Answer:** **6400**.

The third octet spans 256 values (0 through 255) and the fourth octet spans 25 values (101 through 125 inclusive). Therefore, 256 × 25 = 6400 addresses.

## Task 3: Discovering Hosts

ARP resolves an IPv4 address to a MAC address on the local network. If a host wants to send traffic to a device on the same subnet and does not have a cached mapping, it broadcasts an ARP Request. The device that owns the requested address can reply with an ARP Response.

On a local Ethernet network, ARP requests are broadcast to the local segment, so multiple devices may see the request. Only the device that owns the queried IP should normally answer it. Cached ARP entries can mean that a second ping does not need a new ARP exchange.

### Question: “How many devices can see the ARP Request?”

**Answer:** **4**.

The simulation shows the broadcast request being visible to four devices on the local segment.

### Question: “Did computer6 receive the ARP Request? (Y/N)”

**Answer:** **N**.

Follow the visualised packet path in the room. The simulation shows computer6 does not receive that request.

### Question: “How many devices can see the ARP Request?”

**Answer:** **4**.

This is a repeated question in the interactive sequence; use the packet visualisation to confirm the same broadcast count.

### Question: “Did computer6 reply to the ARP Request? (Y/N)”

**Answer:** **Y**.

The second ARP scenario in the room shows computer6 replying to the request. Read the current simulation state carefully because the question refers to a different request from the earlier receive question.

### Question: “What is the type of packet that computer1 sent before the ping?”

**Answer:** **ARP Request**.

Computer1 needs the local MAC address before it can send the Ethernet frame for the ping.

### Question: “What is the type of packet that computer1 received before being able to send the ping?”

**Answer:** **ARP Response**.

The response supplies the IP-to-MAC mapping that the host needs for local delivery.

### Question: “How many computers responded to the ping request?”

**Answer:** **1**.

Only one computer in the simulation responds to the ping request. Do not confuse the number of devices that can see a broadcast ARP request with the number of hosts that reply to the ICMP ping.

### Question: “What is the name of the first device that responded to the first ARP Request?”

**Answer:** **router**.

### Question: “What is the name of the first device that responded to the second ARP Request?”

**Answer:** **computer5**.

### Question: “Send another Ping Request. Did it require new ARP Requests? (Y/N)”

**Answer:** **N**.

The host has cached the ARP mapping from the previous exchange, so it does not need to resolve the same IP address again immediately.

### Question: “How many devices are you able to discover using ARP requests?”

**Answer:** **3**.

This is the result shown by the room's ARP discovery simulation.

## Task 4: Nmap Host Discovery Controls

Nmap offers several probes for determining whether a host is up. A discovery option does not scan every port; it selects a particular probe type.

### ICMP discovery options

- **-PE:** ICMP Echo Request, similar to the probe used by ping.
- **-PP:** ICMP Timestamp Request.
- **-PM:** ICMP Address Mask Request.

### Question: “What is the option required to tell Nmap to use ICMP Timestamp to discover live hosts?”

**Answer:** `-PP`.

### Question: “What is the option required to tell Nmap to use ICMP Address Mask to discover live hosts?”

**Answer:** `-PM`.

### Question: “What is the option required to tell Nmap to use ICMP Echo to discover live hosts?”

**Answer:** `-PE`.

An example of a host-discovery scan using ICMP Echo is:

\`\`\`bash
nmap -sn -PE TARGET_SUBNET
\`\`\`

- **-sn** performs host discovery without a port scan.
- **-PE** uses ICMP Echo Requests.
- **TARGET_SUBNET** is the authorised network range supplied by the room.

## Task 5: TCP and UDP Host Discovery

TCP discovery probes can be useful when ICMP is filtered. A TCP SYN ping sends a SYN packet to a selected port. A TCP ACK ping sends an ACK packet and observes the response. A firewall may treat them differently.

### Question: “Which TCP ping scan does not require a privileged account?”

**Answer:** **TCP SYN Ping**.

### Question: “Which TCP ping scan requires a privileged account?”

**Answer:** **TCP ACK Ping**.

### Question: “What option do you need to add to Nmap to run a TCP SYN ping scan on the telnet port?”

**Answer:** `-PS23`.

An example command is:

\`\`\`bash
nmap -sn -PS23 TARGET_SUBNET
\`\`\`

The **-PS23** option sends TCP SYN discovery probes to TCP port 23, commonly used by Telnet. The command is for the authorised lab range only.

UDP discovery can also be used when the target's network behaviour makes it useful. Nmap's UDP probe option is `-PU`, optionally followed by a port list. A lack of response may be ambiguous because UDP services often do not reply to unexpected packets.

## Task 6: Reverse DNS Lookup

Reverse DNS attempts to map an IP address to a hostname using PTR records. It can help label discovered hosts, but names may be missing, generic, or out of date.

### Question: “We want Nmap to issue a reverse DNS lookup for all the possibles hosts on a subnet, hoping to get some insights from the names. What option should we add?”

**Answer:** `-R`.

Example:

\`\`\`bash
nmap -sn -R TARGET_SUBNET
\`\`\`

The **-R** option forces reverse DNS resolution for target addresses. This may add time to a scan and does not guarantee that every address has a PTR record.

## Troubleshooting and practical notes

- A host that does not answer ICMP may still be online.
- ARP discovery works on the local network segment and is not routed across the Internet.
- Cached neighbour entries can remove the need for another ARP request.
- A TCP SYN probe and TCP ACK probe behave differently and may require different privileges.
- Reverse DNS can return no name or a name that does not match the current host.
- Always use the target range specified by the lab and save scan output for review.

## Key takeaways

Host discovery is about choosing the right probe for the network. ARP is useful on a local Ethernet segment, ICMP probes test IP-level reachability, TCP probes can work when ICMP is filtered, and reverse DNS can add hostnames. Interpret the result in context rather than treating silence as proof that a host is offline.

## Expanded discovery workflow

### 1. Define the authorized range

Use only the lab's assigned subnet or addresses. Confirm the network prefix before scanning; a typo in a CIDR range can expand the scan beyond the intended target. Record the target range, time, and scan options.

### 2. Understand discovery methods

Nmap chooses different host-discovery probes depending on privilege and network context. On a local Ethernet network, ARP discovery can be effective because a host must resolve local link-layer addresses to communicate. Across routed networks, ARP is not forwarded, so ICMP or TCP probes may be more appropriate. Firewalls can filter probes, meaning a machine that is online may not respond.

### 3. Use a small first scan

For an explicitly authorized lab subnet, use the room's stated range. A typical command is:

```bash
nmap -sn -oN discovery.txt TARGET_RANGE
```

`-sn` performs host discovery without a port scan, and `-oN` saves readable output. On a local Ethernet segment, privileged Nmap may use ARP discovery automatically. Exact probe selection depends on the operating system, privileges, and network.

For a target that filters ICMP, the lab may call for TCP discovery probes. Choose the probe based on the room task rather than sending every probe indiscriminately.

### 4. Understand the output

A line such as "Host is up" means Nmap received a response consistent with its discovery method. It does not prove that every port is open. "Host seems down" may mean the target is offline, filtered, or unreachable using the chosen probes.

Reverse DNS can reveal a name associated with an address, but names may be missing or stale. Treat the IP as the primary observed value unless the task specifically asks for a hostname.

### 5. Validate the list

Compare the result with the lab's expected address range. If a host is missing, verify the subnet, interface, privileges, and discovery method. Do not immediately increase scan intensity; first identify the reason the first method may have failed.

### 6. Room answers

For questions asking how many hosts responded, which address was live, or which hostname appeared, derive the answer from the actual saved output. Do not copy a result from a different subnet or session. Redact flags as `flag{hidden}`.
