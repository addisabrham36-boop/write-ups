# Cypheron: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/cypheron

## Room Overview & Information

**Difficulty:** Insane  
**Category:** AI security, software supply-chain compromise, web exploitation, Linux privilege escalation  
**Scope:** Two independent investigations:
1. **Trojaned Model — Neural C2 Beacon**, involving a model artifact, a vendor update mechanism, and suspicious outbound activity.
2. **AI Security and Red Team Nightmare**, beginning at the public intake endpoint /form/file-processor and progressing through an application workflow to user and root flags.

Start each deployment separately and record its current IP. Do not assume that the two tasks share the same target address or state. The five flag values are revealed inside the active room, so this public note identifies the evidence chain without inventing them.

## Phase 1: Reconnaissance & Enumeration

### Challenge 1: Trojaned Model — Neural C2 Beacon

Start the Task 1 machine and record its IP. Scan the active target:

```bash
nmap -sC -sV -oN cypheron-task1-nmap.txt TARGET_IP
```

- **-sC** runs Nmap's default scripts.
- **-sV** probes open services for version details.
- **-oN** saves a readable report.
- Replace TARGET_IP with the live address.

If the room hints that the service is not in the common port range and authorizes a full scan, use:

```bash
nmap -p- --min-rate 1000 -oN cypheron-task1-allports.txt TARGET_IP
```

- **-p-** scans TCP ports 1 through 65535.
- **--min-rate 1000** requests a high minimum packet rate. This can miss results on slow or filtered networks, so use it only when appropriate for the lab.
- **-oN** saves the scan.

Follow the task's service and vendor-update interface. Do not assume ports or endpoints from another deployment.

### Challenge 2: AI Security and Red Team Nightmare

Start the Task 3 machine independently and scan it:

```bash
nmap -sC -sV -oN cypheron-task3-nmap.txt TARGET_IP
```

Open the exposed web service and inspect the application. The briefing identifies the intake route as **/form/file-processor**. Record its method, accepted file types, request fields, response behaviour, and the application state before testing.

## Phase 2: Vulnerability Analysis & Initial Access

### 1. Preserve and inspect the model artifact

The first challenge supplies a model file named **signal_classifier.pt**. Keep an unchanged copy and record its size and hash:

```bash
file signal_classifier.pt
sha256sum signal_classifier.pt
strings -n 6 signal_classifier.pt | head -n 60
```

The commands identify the likely file type, calculate the SHA-256 hash, and extract a bounded sample of readable strings. A PyTorch file may contain tensor data, model state, or serialized Python objects. Do not load an untrusted pickle-based artifact with unrestricted deserialization on a personal machine. Use the isolated lab and a safe inspection method.

The absence of readable strings does not prove that the artifact is clean. Binary tensor data and embedded structures may not be visible through strings.

### 2. Investigate the suspected neural trigger

The scenario describes anomalous outbound communications from inference nodes and asks whether the model contains a hidden trigger. Compare normal classifier behaviour with the evidence supplied by the room. Look for a trigger condition, an unexpected output or side effect, and a relationship between the condition and the suspected network behaviour.

Keep three classes of evidence separate:
- Static evidence in the model artifact or supporting code.
- Behaviour observed when the model is exercised inside the lab.
- Network evidence that demonstrates an actual outbound connection or beacon attempt.

A suspicious string or unusual prediction is not, by itself, proof that a C2 beacon occurred.

### 3. Examine the vendor update mechanism

Trace how the model artifact is selected, transferred, verified, and loaded. Identify missing authentication, absent integrity verification, reliance on an untrusted source, or unsafe deserialization. Distinguish a model backdoor from an unsafe loader:
- A **backdoor** causes unexpected behaviour when a specific trigger condition is met.
- **Unsafe deserialization** may cause code execution while loading a serialized object.

Both weaknesses can coexist but require distinct evidence and mitigations.

### 4. Trace the file-processing workflow in Challenge 2

A file-processing endpoint typically receives input, validates or stores it, then passes it to another component. The security question is whether user-controlled metadata or content can influence a privileged action. Map each transition and identify the account under which the processor runs.

A successful upload is not proof of command execution. Verify the actual processing result and its effect. Use only the provided lab's request and proof steps; do not run payloads against an unrelated service.

## Phase 3: Privilege Escalation & Post-Exploitation

If the second challenge grants a shell or exposes job output, verify the current identity and environment:

```bash
whoami
id
hostname
pwd
ls -la
```

These commands identify the current user, group membership, host, working directory, and accessible entries. A shell running as a web application user is not root. Record the first privilege boundary crossed, then use evidence from the room to investigate the next one. Do not assume a specific SUID, sudo, container, or internal-service path without seeing it on the active target.

If a flag is requested, identify the exact location and read it only after the corresponding privilege level is verified. Do not claim root access based solely on a tool's “success” message.

## Phase 4: Room Questions & Flags Walkthrough

The official task asks for five flags.

### Challenge 1: Trojaned Model — Neural C2 Beacon

1. “What's the first flag?”
2. “What's the second flag?”
3. “What's the third flag?”

Map each flag to the exact artifact, request, model behaviour, or runtime evidence that reveals it. Do not assume all three are printed by one command.

### Challenge 2: AI Security and Red Team Nightmare

1. “Whats the user flag?”
2. “Whats the root flag?”

Start at **/form/file-processor**, trace the workflow, verify the user context, then validate the separate privilege boundary before reading the root flag. The flag values are intentionally redacted as **flag{hidden}** because they must be retrieved from the active instance.

## Phase 5: Key Takeaways & Remediation

For AI model supply-chain risk, use signed artifacts, trusted registries, version pinning, secure build provenance, and approvals for updates. Avoid unsafe deserialization and use safer weights-only formats where supported.

For the file processor, validate both content and metadata, store uploads outside executable web paths, sandbox the converter, restrict egress, use a least-privilege service identity, and authenticate internal workflow endpoints. Preserve hashes, exact requests, observed identities, and relevant outputs. Treat model provenance, execution permissions, file-processing boundaries, and network access as separate controls that must all be secured.