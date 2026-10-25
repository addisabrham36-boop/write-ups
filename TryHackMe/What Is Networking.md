# What Is Networking?: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/whatisnetworking

## Room Overview & Information

**Category:** Networking fundamentals  
**Difficulty:** Beginner  
**Focus:** Networks and the Internet, IP and MAC addresses, MAC spoofing, and ICMP ping.

This room introduces how devices identify one another and communicate. The practicals use simulated networks, so any flag must be obtained from the current room. Do not replace it with a value from an older write-up.

## Task 1: What is Networking?

A computer network is a set of connected devices that can exchange data. Networks range from a few devices on a home LAN to large interconnected networks across the Internet. Devices need agreed rules and addressing to send data to the right destination.

A network is not necessarily the Internet. A private network may connect devices inside a home or organisation, while the Internet connects many networks together through routers and service-provider infrastructure.

## Task 2: What is the Internet?

The Internet is a network of networks. Private networks connect to other networks through routing infrastructure, and protocols allow devices from different manufacturers and operating systems to communicate.

### Question: “Who invented the World Wide Web?”

Answer: **Tim Berners-Lee**.

The World Wide Web is a service built on top of the Internet, using technologies such as HTTP and HTML. The Internet and the Web are related but not identical: email, DNS, and many other services also use the Internet.

## Task 3: Identifying Devices on a Network

Devices commonly use IP addresses and MAC addresses for different purposes.

- **IP address:** a logical address used by IP to identify an interface and route packets.
- **MAC address:** a link-layer address used to deliver frames on a local network.

IPv4 uses four decimal octets separated by dots. Each octet ranges from 0 to 255. IPv6 uses a 128-bit address written in hexadecimal groups.

A private IPv4 address is used inside private networks and is not routed directly across the public Internet. A public address is globally routable, subject to routing and provider policy. Network Address Translation (NAT) commonly lets multiple private devices share one public IPv4 address.

### Question 1: “What does the term ‘IP’ stand for?”

Answer: **Internet Protocol**.

### Question 2: “What is each section of an IP address called?”

For the IPv4 format taught in this room, each dot-separated section is an **octet**.

### Question 3: “How many sections (in digits) does an IPv4 address have?”

Answer: **Four** octets.

### Question 4: “What does the term ‘MAC’ stand for?”

Answer: **Media Access Control**.

## Task 3 practical: MAC spoofing in the simulated hotel network

The room provides an interactive scenario where the router allows Alice's traffic because her device has been authorised, while Bob's traffic is blocked. The exercise asks you to change Bob's simulated MAC address to match Alice's.

1. Open the interactive lab using **View Site**.
2. Inspect the packet flow for Alice and Bob.
3. Note which device's packets are allowed and which are dropped.
4. Use the simulator's control to change Bob's MAC address to the value used by Alice.
5. Send the test traffic again and observe whether the router now allows it.
6. Submit the flag shown by the simulator.

This demonstrates why MAC-address-based access control alone is weak. A MAC address can be spoofed, and it does not provide strong user authentication. The exercise is a simulation; do not spoof addresses on a real network unless you have explicit authorisation.

### Question 5: “Deploy the interactive lab using the ‘View Site’ button and spoof your MAC address to access the site. What is the flag?”

Obtain the flag from the successful simulated action and submit it to the active room. The public write-up deliberately uses `flag{hidden}` rather than publishing a flag value that has not been independently confirmed in the live instance.

## Task 4: Ping (ICMP)

Ping uses ICMP Echo Request and Echo Reply messages to test basic reachability and measure round-trip time. It is useful for troubleshooting, but a failed ping does not prove a host is offline because firewalls can block ICMP.

A Linux example is:

```bash
ping -c 4 8.8.8.8
```

- `ping` sends ICMP echo probes.
- `-c 4` sends four requests and stops.
- `8.8.8.8` is the destination IP in the room's example.

The room's interactive website has its own simulated ping function and reveals a flag when the correct address is pinged. Use that simulator for the question rather than assuming a local terminal result will unlock the task.

### Question 1: “What protocol does ping use?”

Answer: **ICMP** (Internet Control Message Protocol).

### Question 2: “What is the syntax to ping 10.10.10.10?”

Answer: `ping 10.10.10.10`.

This is the basic syntax shown by the room. On Linux, you can limit the number of requests with `-c`, but the question asks for the simple form.

### Question 3: “What flag do you get when you ping 8.8.8.8?”

Open the practical, enter or ping `8.8.8.8` as instructed, and copy the flag shown by the active simulator. Do not substitute a guessed flag from another write-up.

## Troubleshooting

- If the lab does not respond, confirm the simulator has loaded and use the controls it provides.
- If a ping receives no reply on a real network, consider ICMP filtering before concluding the host is offline.
- If a MAC change does not alter the simulation, verify that you copied the authorised device's full address correctly.
- A MAC address is relevant to the local link; routers do not forward the same Ethernet frame unchanged across the Internet.

## Key takeaways

IP addresses support logical addressing and routing; MAC addresses support local-link delivery; and ICMP ping checks a limited form of reachability. The practical demonstrates why trusting a spoofable MAC address is not a strong authentication mechanism.