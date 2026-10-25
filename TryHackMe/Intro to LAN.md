# Intro to LAN — TryHackMe

Room: https://tryhackme.com/room/introtolan

A Local Area Network, or LAN, connects devices within a limited area such as a home, office, or lab. Devices on a LAN can communicate directly when the addressing and network configuration allow it, while a router provides a path to other networks.

## Switching and addressing

A switch forwards Ethernet frames within a LAN using MAC addresses. IP addresses are used for communication at the network layer, and a subnet mask or CIDR prefix helps determine which addresses are local. When a destination is outside the local subnet, a host normally sends the traffic to its default gateway.

I can inspect my own Linux configuration with:

```bash
ip address
ip route
```

The first command shows interface addresses, while the second shows the routing table. These commands inspect the local machine; they do not discover every device on the LAN.

## Why segmentation matters

A network can be divided into separate subnets or VLANs to reduce unnecessary communication between groups of devices. Segmentation can limit the impact of a compromised machine, but it needs appropriate routing and firewall rules to be effective.

## What I took from this room

A LAN is more than devices connected to the same Wi-Fi. I need to understand how MAC addresses, IP addresses, subnets, switches, and gateways work together. These basics are essential before attempting network enumeration or troubleshooting connectivity.