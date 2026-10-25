# Active Reconnaissance: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/activerecon

## Room Overview & Information

**Category:** Network security fundamentals  
**Difficulty:** Beginner  
**Focus:** Browser inspection, ping, traceroute, telnet, netcat, and direct interaction with an authorised target.

Active reconnaissance sends traffic to a target to learn how it responds. Unlike passive research, these actions may appear in server logs, firewalls, intrusion-detection alerts, or honeypot telemetry. The scope must be explicit: use the room's intended targets and do not expand the scan to unrelated systems.

This room focuses on simple network tools. It is not necessary to invent an exploit chain or privilege escalation where the task is teaching reachability and service interaction.

## Phase 1: Establish the scope

Use the target hostname or address supplied by the active room. In examples below, `TARGET_IP` is a placeholder and must be replaced with the room's actual target. A hostname can resolve to a different address over time, so do not blindly reuse an address from an old guide.

Record the target, time, tool, command, and result. This makes it easier to distinguish an observed fact from an assumption.

## Phase 2: Browser and web inspection

A browser makes a normal HTTP or HTTPS request. Inspect the address bar, redirects, certificate information, page content, and developer tools. The Network tab shows the requests made by the page, status codes, headers, and response sizes.

From a terminal, a simple header and response check is:

```bash
curl -i http://TARGET_IP/
```

- `curl` sends an HTTP request.
- `-i` prints the response headers as well as the body.
- The final URL is the target's web service.

If the room specifies a hostname-based site, preserve the hostname and use the provided host mapping or the exact URL. An IP request can reach a default virtual host rather than the intended website.

## Phase 3: Ping and reachability

```bash
ping -c 4 TARGET_IP
```

- `ping` sends ICMP Echo Requests.
- `-c 4` sends four requests and then stops.
- The output reports replies, round-trip times, and packet loss.

A reply demonstrates that ICMP traffic can reach the target and return. No reply is inconclusive: the host may be online while ICMP is filtered.

## Phase 4: Trace the network path

On Linux, use:

```bash
traceroute TARGET_IP
```

If the tool is not installed, the system may provide `tracepath`. Traceroute sends probes with progressively increasing TTL values. Routers that decrement a packet's TTL to zero may return an ICMP time-exceeded message, allowing the tool to infer a hop.

Asterisks do not necessarily mean a router is broken; it may simply not answer the probe. Routing paths can also change between attempts.

## Phase 5: Test a TCP service with Telnet or Netcat

Telnet can make a plain TCP connection to a known service:

```bash
telnet TARGET_IP PORT
```

Replace `PORT` with a port explicitly in scope. A successful connection proves a TCP connection was established, not that the application protocol is understood or that authentication succeeded.

Netcat can make a similar connection:

```bash
nc -nv TARGET_IP PORT
```

- `-n` avoids DNS lookups.
- `-v` prints connection details.
- The target and port identify the service.

For an HTTP service, the client may accept a manually typed HTTP request, but modern HTTPS requires TLS negotiation and cannot be treated as plain HTTP. Use a proper HTTPS client for encrypted endpoints.

## Phase 6: Interpret evidence carefully

- **Connection refused:** the host or an intermediary actively rejected the TCP connection; the port may be closed.
- **Timeout:** filtering, routing issues, or a nonresponsive service may be responsible.
- **Successful TCP connection:** the port accepted a connection, but application-level behaviour still needs inspection.
- **Ping reply:** ICMP reachability worked; it does not prove that every port is open.
- **Traceroute gap:** an intermediate device may be filtering or rate-limiting probes.

Do not interpret one tool's failure as proof of a host's overall state. Combine results and test the specific protocol relevant to the task.

## Phase 7: Question and task workflow

The room teaches these tools through the provided target and browser-based tasks. For each question:

1. Read which protocol or tool the question describes.
2. Use the exact room target, not an arbitrary public host.
3. Run the corresponding test or use the supplied browser.
4. Record the result that the tool actually shows.
5. Match the question to the meaning of the result, not merely to a familiar-looking command.

If a task asks for a path or service response, inspect that exact service. If it asks about a tool's purpose, distinguish reachability testing from route discovery and application interaction.

## Phase 8: Reporting and mitigation

Active recon is a normal part of authorised testing, but it should be rate-limited and scoped. Maintain a written scope, avoid unnecessary scans, and record timestamps. Organisations can reduce unnecessary exposure by closing unused ports, filtering management interfaces, monitoring scan patterns, and documenting approved testing.

## Key takeaways

Ping tests ICMP reachability, traceroute estimates the network path, Telnet and Netcat test TCP connections, and a browser or curl inspects web behaviour. Each tool answers a different question. Active reconnaissance works best when every probe has a clear purpose and the result is interpreted within its limits.

## Expanded practical workflow

### 1. Confirm scope and record the target

Active reconnaissance sends traffic to the target, so begin with the exact hostname or IP permitted by the room. Record the target, start time, and any restrictions. Never scan a public range simply because a hostname resolves to an address.

Resolve the name and record the result:

```bash
getent ahosts TARGET_HOST
```

This uses the host's configured name-resolution path. If the room provides a fixed lab IP, use that value as the source of truth.

### 2. Basic service discovery

A conservative first scan identifies common ports and attempts service/version detection:

```bash
nmap -sC -sV -oN nmap-initial.txt TARGET_IP
```

- `-sC` runs Nmap's default scripts.
- `-sV` probes open ports to identify services and versions.
- `-oN` saves readable output for later comparison.

After reviewing the first result, a full TCP-port scan can reveal services on uncommon ports:

```bash
nmap -p- -T4 -oN nmap-all-tcp.txt TARGET_IP
```

`-p-` checks TCP ports 1 through 65535; `-T4` increases scan speed and should only be used when the lab and network conditions permit it. If a port is found, scan it with relevant scripts and version detection rather than blindly running every script.

### 3. Web enumeration

If HTTP or HTTPS is exposed, inspect the page, response headers, cookies, redirects, robots.txt, and any linked API documentation. Use a directory wordlist appropriate for the target and keep the request rate reasonable:

```bash
gobuster dir -u http://TARGET_HOST/ -w /path/to/wordlist.txt -o gobuster-root.txt
```

The URL must match the scheme and hostname used by the lab. If the application relies on a virtual host, an IP-only request may return a default site. Use the hostname specified by the room and verify it resolves to the target.

A discovered path is not automatically a vulnerability. Open it, inspect its response, and determine whether authentication or authorization is required.

### 4. Enumerate services selectively

Choose enumeration based on what the scan actually found. For SMB, inspect shares and protocol details only if SMB is exposed. For DNS, test the records and zone-transfer behaviour relevant to the supplied domain. For databases, first determine whether the port is reachable and whether the room authorizes authentication testing. For SSH, record the service version and authentication options but do not assume the version alone proves exploitability.

Avoid running tools against services that do not exist. Each command should answer a question raised by the previous result.

### 5. Validate findings and preserve evidence

Save scan output, exact commands, tool versions, and meaningful response excerpts. Distinguish confirmed facts (an open port or returned header) from hypotheses (a suspected vulnerability). Verify any suspected flaw using the smallest safe test allowed by the room.

### 6. Common problems

- If the hostname resolves to the wrong address, check the lab's IP and local hosts mapping.
- If a web scan returns only a default page, verify the Host header and base path.
- If a scan shows no ports, confirm the target is started and the IP is current.
- If results change between scans, consider filtering, service startup, and rate limits.
- If a tool returns an error, read it before changing multiple options at once.

### 7. Answer the room questions

Use the active room's exact target and output. When a question asks for an open port, service, version, hostname, or header, retrieve it from your own scan or request. Do not copy another instance's results. Redact any flag as `flag{hidden}` in public notes.
