# Cypheron

Official room: https://tryhackme.com/room/cypheron

## Room Overview & Information

**Difficulty:** Insane  
**Category:** AI security, software-supply-chain compromise, web exploitation, Linux privilege escalation  
**Scope:** Two independent investigations:
1. **Trojaned Model — Neural C2 Beacon**, involving a model artifact, a vendor update mechanism, and suspicious outbound activity.
2. **AI Security and Red Team Nightmare**, beginning at the public intake endpoint `/form/file-processor` and progressing through an application workflow to user and root flags.

Start each deployment separately and record its current IP. Do not assume that both tasks share a target address or state. The room has five flags. The interactive lab exposes their exact values; this guide explains where to investigate without inventing values that have not been observed.

## Phase 1: Reconnaissance & Enumeration

### Challenge 1: Trojaned Model — Neural C2 Beacon

Start the Task 1 machine and record its IP. Save a service scan:

```bash
nmap -sC -sV -oN cypheron-task1-nmap.txt TARGET_IP
```

- `-sC` runs Nmap's default scripts.
- `-sV` probes discovered ports for service/version information.
- `-oN` writes a readable report.
- Replace `TARGET_IP` with the live address.

If the room's instructions and the observed exposure call for a complete TCP scan, use:

```bash
nmap -p- -T4 -oN cypheron-task1-allports.txt TARGET_IP
```

`-p-` scans TCP ports 1 through 65535; `-T4` chooses a faster timing profile. Read the actual results rather than assuming the service uses a port from another deployment.

### Challenge 2: AI Security and Red Team Nightmare

Start the Task 3 machine separately and save its own scan:

```bash
nmap -sC -sV -oN cypheron-task3-nmap.txt TARGET_IP
```

Open the exposed web service and inspect the application's links and forms. The briefing identifies the file-processing intake route as `/form/file-processor`. Record its HTTP method, content type, form fields, upload restrictions, status codes, and response body. Use the actual route and form shown by the room rather than inventing alternate endpoints.

## Phase 2: Vulnerability Analysis & Initial Access

### 1. Preserve and inspect the model artifact

The first challenge supplies a model file named `signal_classifier.pt`. Preserve the original and record its identity before analysis:

```bash
file signal_classifier.pt
sha256sum signal_classifier.pt
stat signal_classifier.pt
strings -n 6 signal_classifier.pt | head -n 60
```

- `file` identifies the likely format.
- `sha256sum` fingerprints the exact bytes.
- `stat` records filesystem metadata.
- `strings` extracts a bounded sample of printable text.

A PyTorch `.pt` file may contain tensor data, a model state dictionary, or a serialized Python object. Some serialization approaches can execute code during loading. Do not unrestricted-load an untrusted checkpoint on a personal machine. Use the isolated room environment and the safe inspection approach specified by the exercise. A lack of readable strings does not demonstrate that the artifact is clean.

### 2. Investigate the suspected neural trigger

The scenario reports anomalous outbound communications from inference nodes and asks whether the model contains a hidden trigger. Compare normal classifier behaviour with the evidence provided by the room. For each test, record:
- The input supplied to the model.
- The expected classifier result.
- The observed result or side effect.
- The model version and artifact hash.
- Any corresponding network event or log.

Keep three types of evidence separate. Static evidence shows what is present in the artifact or supporting code. Behavioural evidence shows what happens when the model is exercised. Network evidence shows whether a connection or beacon attempt actually occurred. A suspicious string or an unusual prediction is not, by itself, proof of a command-and-control beacon.

### 3. Examine the vendor update mechanism

Trace how the model artifact is selected, downloaded, authenticated, verified, and loaded. Identify whether the update source is trusted, whether the artifact's integrity is verified against a trusted reference, and whether loading it invokes an unsafe deserializer.

A model backdoor and unsafe deserialization are different weaknesses:
- A **backdoor** causes the model to behave unexpectedly when a specific trigger condition is present.
- **Unsafe deserialization** may execute code while a serialized artifact is being loaded.
- A **supply-chain weakness** allows an untrusted or tampered artifact to enter the approved update path.

They can coexist, but each needs its own evidence and remediation. Do not claim the trigger caused a network connection unless the room's logs or observed behaviour establish the relationship.

### 4. Trace the file-processing workflow in Challenge 2

A file processor usually accepts an input, checks it, stores or transforms it, and passes it to another component. Map the path from HTTP request to processing job, output file, or downstream action. Determine whether user-controlled file content or metadata influences a privileged command or internal workflow.

Use Burp Suite or the browser's Network panel to preserve a normal request. Change one field at a time and compare the response. A successful HTTP status only proves that the server returned a response; it does not prove the processor executed a command. If the task provides job output or a shell, verify the effect from observed output.

## Phase 3: Post-Exploitation & Privilege Boundaries

If the second challenge grants shell access or exposes job output, collect the current context before following the next clue:

```bash
whoami
id
hostname
pwd
ls -la
sudo -l
find / -perm -4000 -type f 2>/dev/null
getcap -r / 2>/dev/null
```

These commands:
- Verify the account, group memberships, host, and working directory.
- Show the sudo permissions for the current user.
- Search for SUID files.
- Search for Linux file capabilities.

The last two checks can produce substantial output. Review findings in the context of the room's evidence instead of executing every unusual binary. A shell running as a web application account is not root, and root inside a container is not automatically host root. Verify each privilege transition with `id` and a concrete host-level observation.

Do not assume a particular SUID, sudo, container, or internal-service path unless it appears on the active machine. Follow the task's observed chain and retrieve flags only after confirming the corresponding privilege level.

## Phase 4: Room Questions & Flags Walkthrough

The official room asks for five flags. They are revealed by the interactive deployments rather than being stable text answers.

### Challenge 1: Trojaned Model — Neural C2 Beacon

1. **“What's the first flag?”** Inspect the exact file, update result, or task output associated with the first flag.
2. **“What's the second flag?”** Follow the second task evidence chain and copy the value shown by the lab.
3. **“What's the third flag?”** Verify the model/backdoor or runtime evidence required by the remaining task and copy the displayed value.

Keep each flag mapped to the action or artifact that revealed it. Do not assume all three are printed by one command or that one successful test proves the other two.

### Challenge 2: AI Security and Red Team Nightmare

1. **“Whats the user flag?”** Start at `/form/file-processor`, trace the file-processing workflow, verify initial access, and read the user flag from the location indicated by the room.
2. **“Whats the root flag?”** Validate the separate privilege-escalation path, prove root-level context, then read the root flag.

The flags are represented as `flag{hidden}` in this public copy. Obtain the exact values from the active instance and submit those values there.
