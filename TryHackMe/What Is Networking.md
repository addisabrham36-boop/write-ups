# What Is Networking? TryHackMe Walkthrough

Room: https://tryhackme.com/room/whatisnetworking

Networking is how devices exchange data. A laptop may connect to a website through a home router, several provider networks, and a remote server. Understanding the layers and addressing involved makes it easier to troubleshoot connectivity and investigate security events.

## 1. Identify the main network components

- **Host:** a device that sends or receives network traffic.
- **Switch:** forwards frames between devices on a local network, primarily using MAC addresses.
- **Router:** forwards packets between different IP networks.
- **Access point:** connects wireless clients to a network.
- **Firewall:** permits or blocks traffic according to configured rules.
- **Server:** provides a service such as web hosting, DNS, file sharing, or authentication.
- **Client:** requests a service from a server.

A device can be both a client and a server depending on what it is doing. The terms describe roles, not necessarily separate types of hardware.

## 2. Understand addresses

A MAC address identifies a network interface at the link layer. An IP address identifies an interface logically so packets can be routed between networks. A port number identifies a service or endpoint on a host, such as HTTPS on TCP port 443.

These values answer different questions. A MAC address does not normally travel unchanged across routers, and an IP address alone does not identify which application on the host should receive the data.

Useful local commands on Linux:

```bash
ip addr
ip route
ip neigh
```

- `ip addr` shows interfaces and assigned addresses.
- `ip route` shows routing decisions and the default gateway.
- `ip neigh` shows cached neighbour mappings, commonly IP-to-MAC information on the local link.

The output depends on the machine and network. Do not copy an example address as if it were your own.

## 3. Follow the path to a website

When a user opens a website, the browser typically needs to resolve a domain name through DNS, establish a transport connection, negotiate TLS for HTTPS, and exchange HTTP requests and responses. The network layer routes packets toward the destination, while switches and access points move traffic across local segments.

A simplified sequence is:

1. The device obtains or already has an IP configuration.
2. DNS resolves the hostname to an address.
3. The device routes traffic toward the destination or default gateway.
4. TCP establishes a connection when the application uses TCP.
5. TLS protects the connection for HTTPS.
6. HTTP carries the request and response.

Real deployments may use caching, proxies, VPNs, load balancers, or other mechanisms, so the path is not always a straight line.

## 4. Common protocols and ports

| Protocol | Typical use | Common port |
|---|---|---:|
| DNS | Name resolution | 53 UDP/TCP |
| HTTP | Web traffic | 80 TCP |
| HTTPS | Encrypted web traffic | 443 TCP |
| SSH | Secure remote shell | 22 TCP |
| DHCP | Automatic IP configuration | 67/68 UDP |
| SMTP submission | Sending email | 587 TCP |
| RDP | Remote desktop | 3389 TCP |

A port is a convention, not proof. Services can be configured to use nonstandard ports, and a port scan must be interpreted alongside service detection.

## 5. Test connectivity methodically

Use commands that answer one question at a time:

```bash
ping -c 4 TARGET_IP
```

This tests whether ICMP echo replies are returned. A failed ping does not prove the host is offline because a firewall may block ICMP.

```bash
ip route
```

Check whether a route exists toward the target. For a web service, test the application directly:

```bash
curl -I https://example.com
```

This requests response headers from the named site. If DNS fails, check name resolution; if the route is missing, check network configuration; if the TCP connection works but HTTP returns an error, investigate the application or proxy.

## 6. Security implications

Network segmentation limits how far an attacker can move after compromising one device. Firewalls reduce unnecessary exposure, secure protocols protect data in transit, and monitoring helps identify unusual traffic. Exposed services should be inventoried and restricted to the users and systems that need them.

## What I took from this room

Networking becomes easier when I separate link-layer delivery, IP routing, transport ports, and application protocols. When something fails, I can test each layer rather than repeatedly trying random commands.