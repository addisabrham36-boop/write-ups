# Metasploit: Introduction — Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/metasploitintro

## Room Overview & Information

**Category:** Penetration-testing tools  
**Difficulty:** Beginner  
**Focus:** Metasploit Framework, msfconsole, module search, exploit configuration, payloads, sessions, and result verification.

Metasploit organises security tests into modules. It includes exploit modules, auxiliary modules, payloads, encoders, and post modules. It is not a magic vulnerability scanner: the operator must identify the target, confirm the relevant weakness, choose a suitable module, and configure it correctly. Use only a room target or another system you are explicitly authorised to test.

## Phase 1: Reconnaissance & Enumeration

### 1. Establish a baseline

Before selecting an exploit, identify the active target and its exposed services. A typical lab scan is:

```bash
nmap -sC -sV -oN metasploit-baseline.txt MACHINE_IP
```

- **-sC** runs Nmap's default scripts.
- **-sV** probes discovered ports to identify services and versions.
- **-oN** saves a readable report.
- **MACHINE_IP** must be replaced with the active room address.

A version banner is a lead, not proof of exploitability. Check the relevant service, configuration, and vulnerability preconditions.

### 2. Start Metasploit

Launch the console:

```bash
msfconsole
```

Useful console commands include:

```text
help
version
search type:auxiliary
show exploits
show payloads
exit
```

The help command lists available commands; version displays the framework build; search looks for modules; show exploits and show payloads list the modules in the installed version; exit closes the console. Module availability changes over time, so use the names returned by your local search rather than copying identifiers from an old screenshot.

## Phase 2: Vulnerability Analysis & Module Selection

### 3. Understand the module types

- **Exploit module:** attempts to trigger a specific vulnerability.
- **Auxiliary module:** performs supporting tasks such as scanning, enumeration, or service interaction.
- **Payload:** behaviour delivered after an exploit succeeds.
- **Post module:** performs authorized actions after a session exists.
- **Encoder:** transforms payload bytes for a specific purpose; it does not reliably make code undetectable.

Search by product, service, or vulnerability:

```text
search apache
search type:auxiliary ssh
```

Inspect the candidate before using it:

```text
info MODULE_PATH
use MODULE_PATH
show options
show targets
```

Replace MODULE_PATH with a real module returned by search. The info command explains the module and requirements; use selects it; show options lists settings; show targets lists supported target variants where available.

### 4. Configure the target carefully

Common options include:

| Option | Meaning |
|---|---|
| RHOSTS | Remote target address or range |
| RPORT | Remote service port |
| LHOST | Local callback address for a reverse payload |
| LPORT | Local callback port |
| TARGET | Target implementation or version choice |

Example configuration:

```text
set RHOSTS MACHINE_IP
set RPORT TARGET_PORT
show options
```

Use the IP and port from your own scan. Do not assume the default port is correct. If a payload requires a callback, LHOST must be reachable from the target. The loopback address 127.0.0.1 normally points back to the target, not the AttackBox.

Before running anything, confirm the target is in scope, required options are set, the version matches the module's prerequisites, and target and payload selections are compatible.

## Phase 3: Initial Access, Sessions & Post-Exploitation

### 5. Run and interpret the result

Some modules support a check command that tests whether a target appears vulnerable. Not all modules support it, and a negative result is not always conclusive. When the room instructs you to run the test, use:

```text
run
```

A message that the exploit was sent is not sufficient proof of success. Verify whether a session was opened and inspect the session's actual identity and capabilities.

### 6. Manage sessions

If the module opens a session, Metasploit reports a session identifier. Common console commands are:

```text
sessions
sessions -i SESSION_ID
background
```

Use the actual session ID printed by the framework. sessions lists active sessions; sessions -i interacts with the selected session; background returns to the console without closing the session. Syntax and behaviour can vary by session type and version.

Do not assume a shell runs as root or administrator. Verify identity in the session before claiming elevated privileges. Perform only the post-exploitation steps required by the room.

### 7. Troubleshoot failures

- **No module found:** revise the search terms and inspect the installed module list.
- **Missing required option:** run show options and set the required value.
- **Connection failure:** verify target IP, port, lab state, and VPN/AttackBox connectivity.
- **Exploit failure:** re-check version, architecture, target selection, and prerequisites.
- **No callback:** verify LHOST reachability and match the payload and listener ports.
- **Unexpected privilege level:** confirm identity; exploit success does not imply privilege escalation.

Record the tool version, module path, target options, result, and any error message. Change one assumption at a time instead of repeating the same unsuccessful command.

## Phase 4: Room Questions & Result Verification

The room introduces the framework and its module workflow rather than one fixed exploit chain. For each task:
1. Read the requested module category or console command.
2. Use the installed console to retrieve the exact value.
3. Inspect the module's description and required options.
4. Verify the target and setting before executing.
5. Confirm the result from a real session or observed response.
6. Submit the value requested by the live room.

No fixed exploit result or flag should be invented. Module names and available options can change between framework versions, so the live console and room's current task are the authority.

## Phase 5: Key Takeaways & Remediation

Metasploit is most effective after reconnaissance. Search for a module, read its requirements, configure the correct target, run only the authorised test, and verify the result. Then fix the vulnerability itself: patch affected software, disable unnecessary services, restrict network exposure, run services with least privilege, and monitor exploitation indicators. Keep a reproducible evidence record and close sessions when the task is complete.