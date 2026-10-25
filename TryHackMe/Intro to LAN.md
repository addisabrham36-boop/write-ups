# Intro to LAN: TryHackMe Walkthrough

Room: https://tryhackme.com/room/introtolan

A local area network (LAN) connects devices in a limited area, such as a home, office, classroom, or lab. The devices can communicate directly within the local network, while a router provides a path to other networks.

## 1. LAN building blocks

A LAN may contain computers, servers, printers, switches, wireless access points, and a router. A switch forwards Ethernet frames using MAC addresses. A router connects IP networks and usually acts as the default gateway for devices that need to reach destinations outside the LAN.

A wireless LAN uses Wi-Fi to connect devices to an access point, but the same IP networking concepts still apply. The physical medium differs; the addressing and routing principles remain familiar.

## 2. MAC and IP addresses

A MAC address is used for delivery on the local link. IPv4 addresses identify devices logically and are used by routers to forward packets. The subnet mask or CIDR prefix indicates which addresses are considered part of the local IP network.

For example, `192.168.1.25/24` belongs to the `192.168.1.0/24` network. The `/24` prefix means the first 24 bits identify the network. This is an example only; use the address and subnet shown in the active lab.

Check a Linux machine's configuration:

```bash
ip addr
ip route
ip neigh
```

The first command shows interface addresses, the second shows routes and the default gateway, and the third displays cached neighbour information. If two devices are on the same subnet, they normally communicate directly at the link layer after resolving the destination MAC address. If the destination is on another network, the sender forwards the packet to its router.

## 3. ARP and local delivery

On an IPv4 Ethernet LAN, Address Resolution Protocol (ARP) helps a host discover the MAC address associated with a local IPv4 address. The result is commonly cached so the host does not need to repeat the resolution for every packet.

ARP is a local-link protocol. A router does not simply forward an Ethernet frame unchanged across the internet; each link uses its own frame delivery while the IP packet is routed onward. This distinction helps explain why MAC addresses and IP addresses serve different purposes.

## 4. DHCP and automatic configuration

Dynamic Host Configuration Protocol (DHCP) can provide a host with an IP address, subnet mask, default gateway, DNS resolver, and lease information. If DHCP fails, a device may have no usable address or may not know where to send off-network traffic.

When diagnosing a LAN problem, check whether the interface has an address, whether the subnet looks correct, whether a default route exists, and whether the configured DNS resolver responds.

## 5. Broadcast domains and segmentation

Broadcast traffic is delivered to devices in the relevant Layer 2 broadcast domain. Routers normally separate broadcast domains. VLANs can divide a switched network into separate logical LANs, allowing administrators to isolate guest devices, servers, and user workstations.

Segmentation is a security control because compromise of one host should not automatically provide unrestricted access to every system. Inter-VLAN traffic should be governed by routing and firewall policy.

## 6. Troubleshooting workflow

1. Check the interface state and IP configuration with `ip addr`.
2. Check the routing table with `ip route`.
3. Test the local gateway if permitted by the lab.
4. Test a known reachable IP to distinguish routing from DNS problems.
5. Test a hostname to check name resolution.
6. Test the actual service with an appropriate client such as `curl` or `ssh`.
7. Record the exact error and the point at which the connection fails.

A failed ping is not definitive evidence that a host is down. Firewalls can block ICMP while allowing application traffic. Likewise, a successful ping proves only limited reachability, not that a specific service is working.

## 7. Security checks

Avoid unnecessary open services, use strong authentication, update network equipment, separate guest devices from trusted systems, and monitor unexpected connections. Rogue DHCP servers, ARP spoofing, weak Wi-Fi settings, and flat networks can all undermine LAN security.

## What I took from this room

A LAN is easier to understand when I follow how local frames, IP packets, gateways, and name resolution work together. The same understanding is useful when troubleshooting connectivity and when deciding where network segmentation and monitoring should be placed.