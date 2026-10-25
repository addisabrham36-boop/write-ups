# Active Reconnaissance — TryHackMe

Room: https://tryhackme.com/room/activerecon

Passive reconnaissance uses information that is already public. Active reconnaissance is different because I interact directly with the target to learn how it responds. That can include checking whether a host is reachable, discovering open ports, and identifying services. These actions create traffic and may be logged, so I should only do them against systems I own or have explicit permission to test.

## Start with a target and a scope

In a lab, the target address is usually provided by the room. I should use that exact address instead of guessing an IP from a hostname or copying an address from someone else's walkthrough. For the examples below, `TARGET_IP` is a placeholder that I replace with the lab's assigned address.

Before scanning, I need to know which machines and ports are in scope. An authorised test is not permission to scan everything nearby just because the network interface happens to reach it.

## Checking reachability

A basic connectivity check can be:

```bash
ping -c 4 TARGET_IP
```

This sends four ICMP echo requests. A reply suggests the host is reachable through that path, but no reply does not prove the host is offline. Firewalls may block ICMP, or the network may treat the packets differently.

## Discovering ports with Nmap

Nmap can check for open ports on an authorised target:

```bash
nmap -Pn TARGET_IP
```

The `-Pn` option tells Nmap to skip its usual host-discovery stage and scan the target as if it were online. It can be useful when ICMP or other discovery probes are filtered, but it may also cause Nmap to spend time scanning an address that is genuinely unreachable.

After finding ports, I can investigate service versions and run Nmap's default scripts:

```bash
nmap -sV -sC -oN nmap-initial.txt TARGET_IP
```

Here, `-sV` attempts to identify service versions, `-sC` runs the default script set, and `-oN` saves a normal text report. Script output is a clue, not automatic proof of a vulnerability. I need to understand what a result means before deciding what to test next.

## Reading the results

A port marked `open` means a service accepted a connection during the scan. `closed` generally means the host responded but no service was listening on that port. `filtered` means Nmap could not determine the state because filtering or a lack of response prevented a clear answer.

Service identification helps me choose the next step. For example, an HTTP service may lead to web enumeration, while SSH may suggest checking the room's intended authentication or configuration tasks. I should not jump straight from a port number to a claim that a service is vulnerable.

## What I took from this room

Active reconnaissance gives me direct information about how a target responds, but it is also more visible and intrusive than passive research. I should confirm the scope, use the lab-provided target, save results, and interpret each finding carefully. Nmap is useful because it helps me build an evidence-based map of exposed services instead of blindly guessing what might be running.

The commands above are general examples. I should add exact scan results only after running them in the authorised lab.
