# Metasploit: Introduction — TryHackMe Walkthrough

Official room: https://tryhackme.com/room/metasploitintro

## Room Overview & Information

**Category:** Penetration-testing tools  
**Difficulty:** Beginner  
**Focus:** Metasploit Framework, msfconsole, module search, exploit configuration, payloads, sessions, and verification.

Metasploit organises security tests into modules. It includes exploit modules, auxiliary modules, payloads, encoders, and post modules. It is not a magic vulnerability scanner: the operator must identify the target, confirm the relevant weakness, choose a suitable module, and configure it correctly.

Use only the room's deployed target or another system you are explicitly authorised to test.

## Phase 1: Start the console

Launch the framework:

\`\`\`bash
msfconsole
\`\`\`

Useful console commands include:

\`\`\`text
help
version
search type:auxiliary
show exploits
show payloads
exit
\`\`\`

- **help** lists console commands.
- **version** displays the installed framework version.
- **search** looks for modules.
- **show exploits** lists exploit modules available in the installed build.
- **show payloads** lists payload modules.
- **exit** closes the console.

Module availability changes over time, so use the names returned by your local search rather than copying a module identifier from an old version.

## Phase 2: Understand module types

- **Exploit module:** attempts to trigger a specific vulnerability.
- **Auxiliary module:** performs support tasks such as scanning, enumeration, or service interaction.
- **Payload:** behaviour delivered after an exploit succeeds.
- **Post module:** performs authorised actions after a session exists.
- **Encoder:** transforms payload bytes for a specific purpose; it does not reliably make code undetectable.

Choose a module based on evidence from enumeration. A module that targets the wrong service or version will usually fail and may generate misleading errors.

## Phase 3: Search and inspect a module

Search by product, service, or vulnerability:

\`\`\`text
search apache
search type:auxiliary ssh
\`\`\`

Inspect a candidate module before using it:

\`\`\`text
info MODULE_PATH
use MODULE_PATH
show options
show targets
\`\`\`

Replace MODULE_PATH with a real module name returned by the local search. **info** explains the module, **use** selects it, **show options** lists configuration, and **show targets** lists supported target variants where applicable.

## Phase 4: Configure the target

Common options include:

- **RHOSTS:** remote target address or range.
- **RPORT:** remote service port.
- **LHOST:** local callback address for a reverse payload.
- **LPORT:** local callback port.
- **TARGET:** target implementation or version choice.

Example configuration:

\`\`\`text
set RHOSTS MACHINE_IP
set RPORT TARGET_PORT
show options
\`\`\`

Use the actual IP and port from the room. Do not assume the default port is correct. If the payload requires a callback, LHOST must be an address reachable from the target. The loopback address 127.0.0.1 normally points back to the target itself, not the AttackBox.

## Phase 5: Run and interpret the result

Before launching a module, confirm the target is in scope, the service and version match the prerequisites, required options are set, and the selected target and payload are compatible.

Some modules support a **check** command that tests whether a target appears vulnerable. Not all modules support it, and a negative check is not always conclusive.

When the room instructs you to run the test, the console command is:

\`\`\`text
run
\`\`\`

A successful console message is not enough by itself. Verify whether a session was opened and inspect the session's actual identity and capabilities.

## Phase 6: Manage sessions

If a module opens a session, Metasploit reports a session identifier. Common session commands are:

\`\`\`text
sessions
sessions -i SESSION_ID
background
\`\`\`

Use the actual session ID printed by the framework. **sessions** lists active sessions; **sessions -i** interacts with the selected session; **background** returns to the console without closing the session. Syntax can vary by session type and framework version.

Do not assume a shell runs as root or administrator. Verify the identity inside the session and stop once the lab objective is complete.

## Phase 7: Troubleshooting

- **No module found:** update the search terms and inspect the installed module list.
- **Missing required option:** run **show options** and set the required value.
- **Connection failure:** verify the target IP, port, lab state, and VPN/AttackBox connectivity.
- **Exploit failure:** re-check version, architecture, target selection, and prerequisites.
- **No callback:** check that LHOST is reachable and that the listener and payload ports match.
- **Unexpected privileges:** confirm the identity; exploit success does not imply elevated privileges.

## Phase 8: Remediation and responsible use

Metasploit is a testing framework, not a mitigation. Fix the vulnerability the module targets, patch affected software, disable unnecessary services, restrict network access, and use least privilege. Keep an evidence record of the module, options, target, result, and remediation.

## Key takeaways

Metasploit is most effective after reconnaissance. Search for a module, read its requirements, configure the correct target, run only the authorised test, and verify the result. A tool's output should support a conclusion, not replace reasoning.

## Expanded module workflow and evidence collection

### 1. Establish a baseline

Before choosing a Metasploit module, identify the lab's active target and enumerate its exposed services:

```bash
nmap -sC -sV -oN metasploit-baseline.txt TARGET_IP
```

`-sC` runs Nmap's default scripts, `-sV` probes discovered ports to identify services and versions, and `-oN` saves human-readable results. A version banner is a lead, not proof of exploitability. Confirm the service, configuration, and vulnerability preconditions.

### 2. Launch and inspect the console

Start Metasploit:

```bash
msfconsole
```

Inside the console, search for the vulnerability or service identified in the scan. Inspect a candidate module before using it:

```text
search TYPE_OR_CVE
info MODULE_PATH
use MODULE_PATH
show options
show targets
```

Replace placeholders with the exact search term and module in the lab. `info` explains the module and its requirements; `show options` lists the settings it needs; `show targets` lists supported target types where the module provides them.

### 3. Configure options deliberately

Set the target address and service port from the room's evidence, not from an old walkthrough. If a payload is required, select one compatible with the module and target. Before execution, re-read the options and verify that the target IP is the assigned lab machine.

### 4. Execute and verify

Run the module only when the room calls for it. A message that an exploit was attempted does not prove success. Check whether a session was actually created, interact with it only as instructed, and verify the identity and host context. Separate initial access from privilege escalation; an unprivileged session is not root.

### 5. Record and interpret results

Save the baseline scan, module path, relevant options, tool version, and the meaningful output. Redact credentials and tokens. If the module fails, investigate target reachability, version/configuration differences, required options, and supported targets instead of repeating the same command without a hypothesis.

### 6. Cleanup and mitigation

Close any session when the task is complete and remove artifacts only as the lab instructs. Defenders should patch vulnerable software, disable unnecessary services, restrict network exposure, run services with least privilege, and monitor exploit indicators.

### 7. Room answers

Use the room's current instance to determine exact service versions, module names, options, and flags. Do not invent an exploit result or claim a session was obtained without evidence. Redact public flags as `flag{hidden}`.
