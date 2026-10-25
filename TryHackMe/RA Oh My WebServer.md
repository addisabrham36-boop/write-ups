# Oh My WebServer: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/ohmyweb

## Room Overview & Information

**Category:** Web exploitation, Linux privilege escalation, container-aware enumeration  
**Difficulty:** Medium  
**Key vulnerabilities:** Apache HTTP Server 2.4.49 path traversal/RCE (CVE-2021-41773 and related CVE-2021-42013), an unsafe Linux capability, and the OMIGOD vulnerability (CVE-2021-38647) exposed on an internal host.  
**Goal:** Obtain the user and root flags from the authorised TryHackMe environment.

This machine is a chain of separate trust boundaries. The public web service leads to command execution inside a container. A capability misconfiguration allows privilege escalation inside that container. The container's network then exposes an internal management service that can be exploited to reach the host. Getting root inside a container is not the same as owning the underlying host.

## Phase 1: Reconnaissance and Enumeration

### 1. Start the machine and record the target

Start the room's machine and record the assigned IP. The target address changes between instances, so use `MACHINE_IP` as a placeholder in these notes.

### 2. Scan the exposed services

Run an initial service scan and save the output:

```bash
nmap -sC -sV -oN ohmywebserver-nmap.txt MACHINE_IP
```

- `-sC` runs Nmap's default scripts.
- `-sV` probes open ports to identify service versions.
- `-oN` saves the output in normal text format.
- `MACHINE_IP` is the address supplied by the active room.

The expected high-value findings are SSH on TCP 22 and HTTP on TCP 80, with Apache HTTP Server 2.4.49 reported by the web service. Confirm your own output before relying on this version.

If the initial scan does not explain all exposed TCP services, perform a full TCP port scan:

```bash
nmap -p- --min-rate 1000 -oN ohmywebserver-allports.txt MACHINE_IP
```

- `-p-` scans TCP ports 1 through 65535.
- `--min-rate 1000` asks Nmap to send probes at a minimum rate; use it carefully on slower networks.
- `-oN` saves the results.

A full scan can take longer and is not necessary if the initial scan already identifies the intended services. In this room, the Apache version is the key clue.

### 3. Inspect the web service

Open `http://MACHINE_IP/` in the browser. The page is largely static and does not expose an obvious login or application workflow. Inspect the response headers:

```bash
curl -i http://MACHINE_IP/
```

The `-i` option includes the response headers. A header such as `Server: Apache/2.4.49` supports the version finding, although banners can be altered or patched versions may retain older version strings.

Directory enumeration may be used to map the public site:

```bash
gobuster dir -u http://MACHINE_IP/ -w /usr/share/wordlists/dirb/common.txt -o ohmywebserver-gobuster.txt
```

- `dir` selects directory/file discovery.
- `-u` sets the target URL.
- `-w` selects the wordlist.
- `-o` saves the result.

The primary exploit path is the vulnerable Apache version rather than a hidden application route.

## Phase 2: Vulnerability Analysis and Initial Access

### 4. Research Apache 2.4.49

Apache 2.4.49 contains a path-normalisation vulnerability. Under affected configurations, crafted URL paths can traverse outside the web root. If CGI execution is enabled, the path traversal can be chained into remote command execution.

The relevant identifiers are:
- **CVE-2021-41773:** path traversal in Apache HTTP Server 2.4.49; exploitation impact depends on configuration.
- **CVE-2021-42013:** a related issue affecting Apache 2.4.50, including further path traversal and RCE under affected configurations.

Check the version and prerequisites against a trusted advisory before using a proof of concept. A version banner alone is not sufficient proof that the target is exploitable.

Search the local Exploit-DB index if available:

```bash
searchsploit Apache 2.4.49
```

Searchsploit lists public exploit references. Review the code and its assumptions before using it. Keep any proof of concept restricted to the target deployed by TryHackMe.

### 5. Validate command execution with a harmless command

The room's exploit path uses a crafted request to reach a CGI shell. The critical detail is path normalisation: the client must preserve the encoded path so that the server receives the sequence the vulnerable code mishandles. Tools that normalise the path before sending it can make a valid test fail.

The lab's proof-of-concept request uses `curl --path-as-is` and a CGI path. Use the room's referenced exploit or instructions and first run a harmless identity command such as `whoami` or `id`. Do not begin with destructive commands or a long reverse-shell payload.

A successful response should contain evidence that a command ran in the server's context. The initial execution user is expected to be a low-privilege web-service account, often named `daemon` in this environment. Verify it from the actual output rather than assuming the exploit grants root.

### 6. Obtain an interactive shell if the lab requires it

If the task calls for a reverse shell, use the callback address assigned to your AttackBox and a port permitted by the lab. Start the listener before triggering the exploit. Keep the web server and listener terminals separate, and ensure the target can reach the callback address.

Once a connection arrives, verify the identity and environment:

```bash
whoami
id
hostname
pwd
ip addr
```

- `whoami` identifies the effective user.
- `id` shows UID, GID, and group membership.
- `hostname` identifies the current system or container.
- `pwd` shows the current directory.
- `ip addr` displays network interfaces.

A hostname that looks like a short container identifier and a `.dockerenv` file are clues that the shell is inside a Docker container. Do not assume this shell represents the underlying host.

## Phase 3: Privilege Escalation Inside the Container

### 7. Enumerate Linux capabilities

After obtaining the low-privilege shell, inspect the filesystem and privileges. One of the important checks is Linux capabilities, which grant a process specific privileged operations without necessarily making the file setuid-root.

Run the capability search if the utility is available:

```bash
getcap -r / 2>/dev/null
```

- `getcap` lists file capabilities.
- `-r /` recursively searches from the filesystem root.
- `2>/dev/null` suppresses permission errors so the relevant results are easier to read.

The intended path includes a Python interpreter with the `cap_setuid` capability. That capability allows the process to change its user ID. Granting it to a general-purpose interpreter is dangerous because the interpreter can execute code that uses the privilege.

Check the exact path and capability shown by the target. Do not assume every Python binary on every Linux system has this permission.

### 8. Escalate within the container

The lab's capability misconfiguration permits a Python process to change its UID to 0. Follow the room's intended proof and verify the result:

```bash
whoami
id
```

If the current identity changes to root, you have root inside the container. This does not prove that the host is compromised. The container's namespaces and filesystem boundaries still matter, and the next stage of the room specifically investigates the internal network.

### 9. Locate the user flag

Use the room's instructions to locate the user flag in the compromised environment. Search only within the assigned target and verify the path from the live system. Do not copy the flag from another person's walkthrough.

**Question: “What is the user flag?”**

1. Confirm the current shell and privileges.
2. Locate the user flag using the path or clue supplied by the lab.
3. Read it from your own instance.
4. Submit the exact value to TryHackMe.

This repository intentionally redacts flag values as `THM{hidden}`.

## Phase 4: Internal Network and Host Compromise

### 10. Identify the container network

Inspect the network configuration:

```bash
ip addr
ip route
```

Docker bridge networks often use addresses in the `172.17.0.0/16` range, with the bridge gateway commonly at `172.17.0.1`. The exact addresses depend on the environment. Read the actual route table and do not assume the gateway address without checking.

The point of this stage is to recognise that a container may be able to reach host services that are not exposed through the public interface.

### 11. Identify the internal OMI service

The room's final stage involves Microsoft Open Management Infrastructure (OMI) and CVE-2021-38647, known as **OMIGOD**. OMI is a management agent used on some Linux systems. The vulnerability allowed unauthenticated remote code execution under affected configurations.

The internal service is not the same as the public Apache service. Use the room's intended internal target and evidence to identify the service, then confirm that the version/configuration matches the vulnerability. Do not scan unrelated networks.

### 12. Use the supplied OMIGOD proof of concept

The room references a public proof of concept for CVE-2021-38647. Review the script and use it only against the authorised internal target in the lab. The general invocation pattern is:

```bash
python3 CVE-2021-38647.py -t INTERNAL_TARGET -c 'whoami; id; hostname'
```

Replace `INTERNAL_TARGET` with the internal address identified from the active lab and use a harmless command sequence first. The exact script filename and options should match the version provided by the room or its referenced source.

A successful response running as UID 0 demonstrates command execution in the OMI host's context. Verify the result before retrieving the final flag. Do not assume that a failed attempt means the service is safe; check target address, network reachability, version, and proof-of-concept prerequisites.

### 13. Retrieve the root flag

**Question: “What is the root flag?”**

1. Confirm the OMI exploit ran on the intended internal host.
2. Verify the execution identity using `whoami` or `id`.
3. Locate the root flag at the path specified by the room.
4. Read it from your live target and submit it to TryHackMe.

The root flag is not reproduced in this public write-up.

## Phase 5: Root Causes, Mitigation, and Lessons

### Apache path traversal/RCE

- Upgrade Apache to a fixed supported version.
- Disable CGI if it is not required.
- Restrict access to sensitive paths.
- Monitor for suspicious encoded traversal requests and unexpected child processes.

### Dangerous Linux capabilities

- Remove `cap_setuid` from general-purpose interpreters.
- Audit file capabilities with `getcap -r /`.
- Run services with minimal permissions.
- Use container hardening and avoid treating container root as a safe security boundary.

### Internal OMI exposure

- Patch or remove vulnerable OMI installations.
- Restrict management services to trusted networks.
- Enforce authentication and network segmentation.
- Monitor management interfaces for unexpected commands and connections.
- Prevent web containers from reaching host management services unless explicitly required.

## Attack-chain summary

```text
Nmap service detection
        |
        v
Apache 2.4.49 identified
        |
        v
CVE-2021-41773 / CVE-2021-42013
        |
        v
Command execution as low-privilege web user
        |
        v
Linux capability enumeration
        |
        v
cap_setuid on Python -> root inside container
        |
        v
Inspect container network and internal services
        |
        v
OMIGOD (CVE-2021-38647) on internal OMI service
        |
        v
Root-level command execution on the internal host
```

## Key takeaways

Oh My WebServer is not a single exploit. It chains a vulnerable web server, a dangerous Linux capability, and an internal management-service vulnerability. Verify each stage separately, distinguish container root from host root, and obtain both flags from the active room. The durable fixes are patching, least privilege, removal of unsafe capabilities, and strict network segmentation.