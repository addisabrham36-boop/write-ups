# Nmap Live Host Discovery — TryHackMe

Room: https://tryhackme.com/room/nmap01

Before scanning ports, I need to work out which hosts are alive. Nmap has a host-discovery stage that uses different probes to decide whether a target appears reachable. This is useful when checking a lab network, but the result is not a perfect list of every machine that exists. Firewalls and network rules can hide hosts from particular probes.

## Basic discovery

For a single authorised target, I can try:

```bash
ping -c 4 TARGET_IP
nmap -sn TARGET_IP
```

The first command sends four ICMP echo requests. The second asks Nmap to perform host discovery without a port scan. `-sn` means skip the port scan, not skip all network activity.

For an authorised range, I replace the example range with the exact subnet given by the lab:

```bash
nmap -sn 192.0.2.0/24
```

The address range above is reserved for documentation, so it is only an example. I must not scan a real network unless it is explicitly in scope.

## Why different probes matter

On a local Ethernet network, Nmap may use ARP discovery. On other networks it can use ICMP or TCP probes, depending on privileges and options. A host may ignore ping but still have a service reachable on a TCP port. That is why a missing ping response does not prove a host is down.

If I need to investigate a target that blocks ordinary discovery, I can tell Nmap to treat it as online and proceed with a scan:

```bash
nmap -Pn TARGET_IP
```

This does not magically make the host reachable. It simply skips the normal host-discovery decision, so it may waste time if the target really is offline.

## Reading the output

Nmap reports hosts it considers up and may show their latency or MAC address when available. The details depend on the network and scan method. I should record what the output actually says rather than assuming that every device will appear.

## What I took from this room

Host discovery is an early step in network reconnaissance. It helps narrow down which systems to investigate before I spend time scanning ports. The important lesson is to understand the probe being used, keep the target range within scope, and remember that filtered traffic can make a live host look absent.