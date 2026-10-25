# Nmap Live Host Discovery — TryHackMe

Room: https://tryhackme.com/room/nmap01

This room focuses on discovering which hosts are online before scanning their services. A live-host scan is not the same as a port scan: host discovery tries to determine whether a target responds, while port scanning checks which services are listening.

I only run these commands against the lab network and targets the room assigns. I use `TARGET_IP` and `TARGET_SUBNET` as placeholders because TryHackMe addresses can change between sessions.

## 1. Check the lab connection

1. Start the machine or network required by the task.
2. Confirm that my AttackBox or VPN is connected.
3. Copy the target address or subnet from the room.
4. If I am unsure what local routes exist, check my own machine:

   ```bash
   ip address
   ip route
   ```

These commands show my local interface addresses and routing table. They do not scan the target network.

## 2. ICMP echo discovery

The simplest host-discovery attempt is an ICMP echo request:

```bash
ping -c 4 TARGET_IP
```

The `-c 4` option sends four requests and then stops. Replies suggest that the host is reachable and responding to ICMP. No reply does not prove the host is down: firewalls may block echo requests, the route may be wrong, or the machine may not be ready.

## 3. Nmap ping scan

Nmap's `-sn` option performs host discovery without a port scan:

```bash
nmap -sn TARGET_SUBNET
```

Replace the subnet with the CIDR range assigned by the room. For example, the format `192.0.2.0/24` means a 256-address IPv4 range, but that example range is documentation-only and must not be substituted for the real lab network.

Read the output for hosts Nmap reports as up. On a local Ethernet network, Nmap may use ARP discovery. Across routed networks, it may use ICMP or TCP-based probes depending on privileges and options.

## 4. Understand the discovery probes

Nmap can use different probes because networks filter traffic differently:

- **ICMP echo:** asks the host to reply to an echo request.
- **TCP SYN or ACK probes:** test whether a host responds to a TCP packet sent to a chosen port.
- **UDP probes:** send UDP packets and look for responses such as ICMP port-unreachable messages.
- **ARP discovery:** useful on the local Ethernet segment because hosts normally need to respond to ARP for local IP resolution.

The exact probe set depends on the command, network position, privileges, and Nmap version. I should choose the method described by the room rather than assume every network permits ICMP.

## 5. Try TCP-based discovery

If ICMP is filtered, a TCP probe to a likely open port can still provide a response:

```bash
nmap -sn -PS22,80,443 TARGET_SUBNET
```

The `-PS` option sends TCP SYN discovery probes to the listed ports. This is still a host-discovery scan; it does not perform a full port scan. Use only the ports and subnet allowed by the lab.

A TCP RST or SYN/ACK response can indicate that a host is present even when it ignores ICMP echo requests. If the port is filtered, the result may remain ambiguous.

## 6. Compare the methods

When the task asks me to compare discovery methods, I run the required command against the same lab scope and note which addresses appear in each result. Differences are expected because hosts and firewalls can respond differently to different probe types.

I should not conclude that a host is offline just because one scan did not discover it. The correct conclusion is narrower: the host did not respond to the probes used under those network conditions.

## 7. Read and save the output

Save the result when I need to compare scans:

```bash
nmap -sn -oN live-hosts.txt TARGET_SUBNET
```

The output file records the discovered hosts and gives me something to review later. If a task asks for the number of hosts, count the hosts reported as up, not the number of lines in the whole report.

## Troubleshooting

- **No hosts discovered:** confirm the machine is running, the IP range is correct, and the VPN route exists.
- **Ping fails:** try the Nmap method required by the room; ICMP can be blocked.
- **Scan seems to take too long:** check that the subnet is the one assigned by the lab and not a much larger range.
- **Different scans show different hosts:** compare which probes each scan uses and whether a firewall may be filtering them.
- **Nmap says the host is down:** use `-Pn` only when the task calls for skipping host discovery and scanning a known target. It tells Nmap to treat the host as online; it does not prove the host is actually reachable.

## What I took from this room

Host discovery is the first step in mapping a network, but its results depend on the probes and network controls. A careful scan uses the correct scope, saves the output, and treats “no response” as an observation rather than proof that a host does not exist.

Reference: https://tryhackme.com/room/nmap01
