# AI Forensics

Official room: https://tryhackme.com/room/aiforensics

## Room Overview & Information

**Category:** AI security and digital forensics  
**Objective:** Preserve and examine AI-related artifacts, interpret model and data evidence, correlate logs, and report findings without overstating what the evidence proves.

AI forensics is not limited to deciding whether a model produced a bad answer. Relevant evidence can include model weights, configuration files, tokenizer data, training datasets, prompts, inference logs, tool calls, package metadata, host logs, and network activity. This is a learning room, so host enumeration, initial access, and privilege escalation only apply if a task explicitly supplies a target that requires them.

## Phase 1: Reconnaissance & Evidence Enumeration

### 1. Preserve the supplied artifacts

Start the room environment and read the task instructions before opening the supplied files. Record filenames, sizes, hashes, timestamps, and sources. Work from copies whenever practical.

```bash
mkdir -p ai-forensics/{evidence,working,notes}
file /path/to/ARTIFACT
sha256sum /path/to/ARTIFACT
stat /path/to/ARTIFACT
```

Replace the placeholder path with the actual room file. **file** identifies a likely format; **sha256sum** fingerprints the exact bytes; **stat** reports filesystem metadata. A matching hash shows two copies are byte-identical, not that the file is benign or authentic.

### 2. Identify the file format

A filename extension can be misleading. Inspect the header and readable strings where appropriate:

```bash
xxd -l 64 /path/to/ARTIFACT
strings -n 6 /path/to/ARTIFACT | head -n 80
```

xxd displays the first 64 bytes as hexadecimal; strings extracts printable sequences, and head limits the output. Neither command fully understands the format. Do not load an untrusted pickle or PyTorch artifact using unrestricted deserialization on a personal machine.

## Phase 2: Artifact Analysis & Initial Findings

### 3. Analyse AI-specific evidence

Identify what each artifact should contain. A model bundle may include weights, configuration, tokenizer files, and metadata. A prompt transcript may include system/developer instructions, user input, retrieval results, tool calls, and output. Compare the observed structure with the expected release or task description.

For logs, preserve timestamps and session identifiers. Determine whether a suspicious result is associated with a particular input, model version, tool invocation, or retrieval source. A model producing harmful or inaccurate text is not, by itself, proof that the model or host was compromised.

### 4. Distinguish potential root causes

Separate these hypotheses because the evidence required differs:

- **Prompt injection:** untrusted content changes the model's behaviour.
- **Training-data poisoning:** manipulated examples influence learned behaviour.
- **Model backdoor:** a particular trigger causes an unexpected response.
- **Unsafe tool integration:** the model can invoke a tool without appropriate authorization.
- **Supply-chain tampering:** a model, dataset, or dependency was altered before deployment.
- **Host compromise:** the system running the model was accessed or modified without authorization.

Correlate the observed behaviour with an artifact, configuration, log, or reproducible test before choosing a conclusion.

### 5. Build an evidence timeline

| Evidence | Record | Limitation |
|---|---|---|
| Original artifact | Filename, size, SHA-256 | Hash does not prove authenticity |
| Metadata | Timestamps, owner, format | Metadata may be copied or altered |
| Model behaviour | Input, output, model version | One output may not be repeatable |
| Tool invocation | Arguments, authorization result | A response does not prove action succeeded |
| Network or host logs | Timestamp, process, endpoint | Logs may be incomplete or unsynchronized |

Keep observed facts separate from interpretations. State the confidence level and identify missing evidence.

## Phase 3: Validate Findings & Post-Investigation Handling

Repeat the smallest safe test needed to check the hypothesis. Change one variable at a time and preserve the exact input and output. If the task concerns a network callback, verify whether an actual connection occurred; if it concerns a tool action, confirm the backend state changed; if it concerns a hash, calculate it from the exact artifact.

This room does not inherently require a root shell or host privilege escalation. Do not claim access or compromise unless the live task provides evidence. Preserve the original sample, keep untrusted code in the isolated room environment, and avoid uploading confidential files to public analysis services.

## Phase 4: Room Questions & Evidence Walkthrough

For each room question:
1. Identify the artifact or observation the question refers to.
2. Locate the exact source field, log line, model configuration, or tool result.
3. Verify the value against the original evidence.
4. Record the command/tool and relevant output.
5. State what the evidence proves and what remains uncertain.

If a task requests a hash, preserve the full digest. If it requests a filename, retain the exact spelling. 
