# Networking Concepts: TryHackMe Walkthrough

Room: https://tryhackme.com/room/networkingconcepts

Networking concepts become easier to use when I separate local-link delivery, IP routing, transport protocols, and application services. Each layer answers a different question, and troubleshooting is faster when I test them in order.

## 1. Network layers

The OSI model divides communication into seven layers: Physical, Data Link, Network, Transport, Session, Presentation, and Application. The TCP/IP model groups these functions into fewer layers. The models are useful for describing responsibilities, even though real implementations do not always fit neatly into a diagram.

- Ethernet and Wi-Fi handle local-link communication.
- IP addresses and routing move packets between networks.
- TCP provides a reliable, ordered byte stream; UDP sends datagrams without the same delivery guarantees.
- Application protocols such as HTTP, DNS, and SSH provide specific services.

## 2. Addressing and ports

A MAC address is used for local-link delivery. An IP address identifies a network interface logically. A port identifies an application endpoint on a host. For example, a web service commonly listens on TCP 80 or 443, while SSH commonly uses TCP 22.

These are conventions, not guarantees. A service may use a nonstandard port, and the port alone does not prove which application is running.

## 3. Subnets and routing

A subnet prefix describes which bits of an IP address identify the network. Devices in the same subnet can often communicate directly on the local link. To reach a different subnet, a host normally sends traffic to a router or default gateway.

On Linux, inspect the local configuration with:

    ip addr
    ip route
    ip neigh

ip addr shows interface addresses, ip route shows routing decisions, and ip neigh displays cached neighbour mappings. Use the output from the actual lab rather than copying example addresses.

## 4. DNS and application protocols

DNS resolves names to records such as A, AAAA, MX, and TXT. A client usually asks a configured resolver, which may answer from cache or query the DNS hierarchy. If a hostname fails but the destination IP is reachable, investigate name resolution separately from routing.

For an HTTP service, curl can help inspect the response:

    curl -i http://TARGET_IP/

Replace TARGET_IP with the lab address and use the correct port and hostname. The status code, headers, and body can help distinguish an application error from a network failure.

## 5. TCP, UDP, and firewalls

TCP establishes a connection and provides reliable, ordered delivery. UDP has less connection overhead and is used by protocols that can tolerate loss or implement their own reliability. DNS can use both TCP and UDP depending on the query and response.

A firewall can allow or block traffic based on addresses, ports, protocols, and connection state. A blocked ping does not prove that a host is offline; ICMP may be filtered while TCP services remain available.

## 6. Troubleshooting sequence

1. Check whether the interface is up and has an address.
2. Check the subnet and default route.
3. Test reachability to the gateway if permitted.
4. Test a known destination IP.
5. Resolve the hostname and inspect the DNS result.
6. Connect to the specific service and read its error.
7. Check local firewall, remote filtering, and application logs where appropriate.

This sequence separates link and routing problems from DNS and application problems. Record the exact error rather than writing only “the network is broken.”

## 7. Working through the room

Match each question to the relevant layer or protocol. If the task describes local delivery, think about MAC addresses and the data-link layer; if it describes forwarding between networks, think about IP routing; if it describes a reliable stream, think about TCP; and if it describes name resolution, think about DNS.

## What I took from this room

Layered thinking prevents random troubleshooting. Each command should answer a specific question about addressing, routing, transport, or the application, and the result should guide the next step.