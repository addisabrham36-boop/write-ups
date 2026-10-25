# Networking Concepts — TryHackMe

Room: https://tryhackme.com/room/networkingconcepts

Networking is how devices communicate and share services. Understanding the basics makes it easier to troubleshoot a connection and understand what a security tool is actually checking.

## Addresses and ports

An IP address identifies a network interface at the IP layer. A port number helps the operating system direct traffic to the right service. For example, web services commonly use ports 80 and 443, while SSH commonly uses 22. These are conventions, not guarantees; services can run on other ports.

TCP establishes a connection and provides ordered, reliable delivery. UDP is connectionless and has less protocol overhead, but applications must handle any reliability they need themselves.

## Local networks and routing

A subnet describes a range of IP addresses. A default gateway is the router a host uses when sending traffic outside its local network. DNS resolves names to records, while DHCP can automatically provide a host with network configuration such as an IP address, gateway, and DNS server.

On Linux, I can inspect local configuration with:

```bash
ip address
ip route
```

These commands show interface addresses and the routing table. They do not scan other hosts or prove that an application service is reachable.

## Troubleshooting a connection

I should check the problem in layers: does the interface have an address, is there a route, can the destination be reached, does DNS resolve the name, and is the expected port accepting connections? A failure at one layer can look like a problem somewhere else.

## What I took from this room

IP addresses, ports, TCP/UDP, routing, DNS, and DHCP are foundational concepts. They help me reason about where traffic should go and where it might be blocked. Instead of treating a failed connection as one mysterious problem, I can test each layer and narrow down the cause.