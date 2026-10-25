# Intro to LAN: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/introtolan

## Room Overview & Information

**Category:** Networking fundamentals  
**Difficulty:** Beginner  
**Focus:** LAN topologies, switches and routers, subnetting, ARP, and DHCP.

A Local Area Network (LAN) connects devices within a limited area, such as a home, office, classroom, or lab. This room introduces the physical and logical pieces that allow devices to communicate and then uses interactive diagrams to reinforce the concepts.

## Task 1: LAN Topologies

A network topology describes how devices are connected.

### Star topology

Each device connects to a central switch or hub. It is easy to add devices and troubleshoot individual links, but the central device is a point of failure. It also requires more cabling than a simple bus design.

### Bus topology

Devices share a backbone cable. It is inexpensive and uses less cabling, but faults on the backbone can disrupt the network and troubleshooting becomes difficult as the network grows.

### Ring topology

Devices form a loop and data travels around the ring. The design can provide predictable paths, but a single break may disrupt a basic ring unless redundancy is built in.

### Switches and routers

A **switch** connects devices within a LAN and forwards Ethernet frames using MAC addresses. A **router** connects different IP networks and forwards packets based on routing information.

### Task 1 questions

**Question: “What does LAN stand for?”**

Answer: **Local Area Network**.

**Question: “What is the verb given to the job that Routers perform?”**

Answer: **Routing**. Routing is the process of selecting a path for packets between networks.

**Question: “What device centrally connects multiple devices and transmits data to the correct location?”**

Answer: **Switch**. A switch learns which MAC addresses are reachable through its ports and forwards frames toward the intended destination.

**Question: “What topology is cost-efficient to set up?”**

Answer: **Bus Topology**. A basic bus design uses a shared backbone and less cabling, though it has reliability and troubleshooting disadvantages.

**Question: “What topology is expensive to set up and maintain?”**

Answer: **Star Topology**. Each endpoint needs a link to the central device, increasing cabling and central-device requirements.

### Interactive topology flag

The task includes an interactive topology diagram. Open it using the room's **View Site** control, inspect the topology and the described failure, then answer using the simulator's feedback. The flag is deliberately redacted here as `THM{hidden}`; retrieve it from the active room rather than copying a flag from another instance.

## Task 2: Subnetting

Subnetting divides a larger IP network into smaller logical networks. It helps organise addresses, control traffic, and separate systems by role or trust level.

An IPv4 address contains 32 bits, usually written as four decimal octets. Each octet ranges from 0 to 255. A subnet mask or CIDR prefix identifies which bits belong to the network portion.

For example, `192.168.1.100/24` is an illustrative host address in the `192.168.1.0/24` network. The `/24` prefix means the first 24 bits identify the network. This is an example only; use the values from the active task.

Three important address roles:
- **Network address:** identifies the network itself.
- **Host address:** identifies a device within the network.
- **Default gateway:** the router a host normally uses to reach another network.

### Task 2 questions

**Question: “What is the technical term for dividing a network into smaller pieces?”**

Answer: **Subnetting**.

**Question: “How many bits are in a subnet mask?”**

Answer: **32** for IPv4.

**Question: “What is the range of a section (octet) of a subnet mask?”**

Answer: **0-255**.

**Question: “What address identifies the start of a network?”**

Answer: **Network Address**.

**Question: “What address identifies devices within a network?”**

Answer: **Host Address**.

**Question: “What is the device responsible for sending data to another network called?”**

Answer: **Default Gateway**.

## Task 3: ARP Protocol

Address Resolution Protocol (ARP) allows an IPv4 host to discover the MAC address associated with an IPv4 address on the local network.

A typical exchange works like this:
1. Host A needs to send data to a local IPv4 address.
2. If it does not have a cached mapping, it broadcasts an ARP Request asking which device owns that IP.
3. The target device replies with an ARP Reply containing its MAC address.
4. Host A stores the mapping in its ARP cache and uses it to deliver Ethernet frames.

ARP is used on the local link. It does not replace IP routing, and routers do not forward the same Ethernet frame unchanged across the Internet.

### Task 3 questions

**Question: “What does ARP stand for?”**

Answer: **Address Resolution Protocol**.

**Question: “What category of ARP packet asks a device whether it has a specific IP address?”**

Answer: **Request**. An ARP Request asks which device owns the target IPv4 address.

**Question: “What address is used as a physical identifier for a device?”**

Answer: **MAC Address**.

**Question: “What address is used as a logical identifier for a device?”**

Answer: **IP Address**.

## Task 4: DHCP Protocol

Dynamic Host Configuration Protocol (DHCP) automatically provides network configuration to clients. It can assign an IP address, subnet mask, default gateway, DNS resolver, and lease duration.

The common four-step exchange is called DORA:

1. **Discover:** the client broadcasts to locate a DHCP server.
2. **Offer:** a server offers configuration to the client.
3. **Request:** the client requests the offered address.
4. **ACK:** the server acknowledges the lease.

DHCP reduces manual configuration, but rogue DHCP servers can provide malicious gateway or DNS settings. Managed networks should restrict who can provide DHCP service.

### Task 4 questions

**Question: “What type of DHCP packet does a device use to retrieve an IP address?”**

Answer: **DHCP Discover**.

**Question: “What type of DHCP packet does a device send once offered an IP address?”**

Answer: **DHCP Request**.

**Question: “What is the last DHCP packet sent from a server to a device?”**

Answer: **DHCP ACK**.

## Troubleshooting and verification

If the interactive diagram does not respond, confirm the lab has loaded and follow the on-screen instructions. For a real Linux host, these commands help inspect network configuration:

\`\`\`bash
ip addr
ip route
ip neigh
\`\`\`

- **ip addr** displays interface addresses.
- **ip route** displays routes and the default gateway.
- **ip neigh** displays cached neighbour mappings, including IP-to-MAC information.

A successful ping does not prove that every service is available. Firewalls can block ICMP while allowing other protocols.

## Key takeaways

LAN topologies describe how devices connect; switches forward frames within a LAN; routers connect networks; subnetting divides address space; ARP maps local IPv4 addresses to MAC addresses; and DHCP distributes network configuration. These basics support both troubleshooting and secure network design.