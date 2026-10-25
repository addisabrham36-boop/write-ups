# What Is Networking? — TryHackMe

Room: https://tryhackme.com/room/whatisnetworking

A computer network is a group of devices that can exchange data. That can be two computers on a home Wi-Fi network or servers communicating across the internet. The point is to let devices share information and services without needing every machine to work alone.

## The basic pieces

An IP address identifies a network interface at the IP layer. A MAC address identifies a network interface at the local-link layer. A port number helps the operating system direct traffic to an application or service. These are related, but they are not interchangeable.

Routers move packets between networks. Switches connect devices within a local network. DNS resolves domain names to records, and DHCP can assign network settings automatically.

## Protocols

TCP provides reliable, ordered delivery using a connection. UDP sends datagrams without the same connection and delivery guarantees. HTTP and HTTPS are commonly used for web traffic, while DNS supports name lookups and SSH is often used for secure remote administration.

A port number is only a clue about the service. A service can run on a nonstandard port, and seeing a port open does not prove that the service is vulnerable.

## What I took from this room

Networking becomes easier when I separate addressing, routing, ports, and protocols. When a connection fails, I can check each layer instead of treating the whole network as one mysterious box.