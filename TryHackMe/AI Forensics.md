# AI Forensics: TryHackMe Walkthrough

Room: https://tryhackme.com/room/aiforensics

## Room Overview & Information

This note covers the practical workflow, evidence to collect, common interpretation errors, and defensive lessons for AI Forensics. Use the live room as the authority for task wording and machine-specific values. The repository does not include fabricated terminal output, credentials, or flags.

## 1. Lab setup and evidence handling

Start the room's virtual environment and read the task brief before touching the artefacts. Record the supplied filenames, hashes if provided, timestamps, and any stated constraints. In a forensic investigation, preserving the original matters because opening, converting, or re-saving a file can change metadata.

Create a working directory and record evidence hashes:

```bash
mkdir -p ai-forensics/{evidence,working,notes}
cd ai-forensics
sha256sum /path/to/ROOM_ARTIFACT
file /path/to/ROOM_ARTIFACT
stat /path/to/ROOM_ARTIFACT
```

Replace the placeholder path with the actual room artifact. `sha256sum` fingerprints the bytes, `file` identifies likely file formats, and `stat` records filesystem metadata. A hash proves whether a copy changed; it does not prove the artifact is benign or authentic.

Copy the artifact into the evidence folder and work on a separate copy. Preserve the original file read-only where the lab permits it. Write down the tool, version, command, and relevant output for every transformation.

## 2. Establish what the artifact is

Use the filename only as a hint. An extension can be wrong or intentionally misleading. Start with `file`, then inspect the first bytes where appropriate:

```bash
xxd -l 64 /path/to/ROOM_ARTIFACT
strings -n 6 /path/to/ROOM_ARTIFACT | head -n 80
```

`xxd` shows a small hexadecimal header; `strings` extracts readable sequences from binary data. Neither command completely understands the format. A lack of readable strings is not evidence that a file is safe.

If the room provides a model, dataset, prompt log, image, or serialized object, identify the expected format before choosing a parser. Do not load untrusted Python pickle or PyTorch artifacts with unrestricted deserialization on a personal machine. Use the isolated lab or a safe inspection approach specified by the room.

## 3. Build an evidence timeline

For each artefact, note its source, creation or modification time when available, hash, and what was learned. Compare timestamps cautiously: file metadata can be copied or altered, and system clocks may differ.

| Evidence | What to record | Why it matters |
|---|---|---|
| Original file | Name, size, SHA-256 | Identifies the exact bytes examined |
| Metadata | Timestamps, owner, format | Provides context, not proof by itself |
| Tool output | Tool/version and relevant lines | Makes findings reproducible |
| Behaviour | Input, action, observed result | Connects a hypothesis to a test |
| Network or logs | Timestamp, endpoint, event | Helps corroborate suspicious activity |

Do not treat a single anomaly as conclusive. A suspicious string, an unexpected model output, or an unusual timestamp is a lead. Correlate it with a second source where possible.

## 4. Investigate AI-specific artefacts

AI systems add evidence types that ordinary file forensics may not cover: model weights, tokenizer/configuration files, training data, prompts, tool calls, inference logs, and model-serving telemetry. Ask what each artefact is supposed to contain and what would be unexpected.

For a model bundle, compare configuration and weight filenames, check the source and checksum if the room provides them, and look for unexplained extra files. For prompt or inference logs, preserve timestamps and session identifiers, then determine whether suspicious behaviour is tied to a particular input, model version, or tool invocation.

A model that produces a malicious or inaccurate answer is not automatically proof of compromise. Distinguish between a prompt that steers normal behaviour, a poisoned or backdoored model, an unsafe tool integration, and a compromised host. The evidence needed for each conclusion is different.

## 5. Answering the room questions

Read each question literally and identify the artefact that can answer it. Search for the relevant metadata, string, log entry, model configuration, or behavioural observation; then verify the answer against the source. If the room asks for a hash, preserve its exact character case and length. If it asks for a filename or value, do not add punctuation copied from a terminal prompt.

The actual room answers can depend on the provided files, so retrieve them from the active lab. In this public write-up, record the method and redact any flag as `flag{hidden}` rather than publishing it.

## 6. Reporting and remediation

A useful forensic note states the observation, the evidence supporting it, the conclusion, and the confidence level. Keep facts separate from hypotheses. Include the exact command used and a small, truthful output excerpt from your own run. Never fabricate terminal output to make a write-up look complete.

For AI pipelines, maintain model provenance, sign or hash release artefacts, validate data sources, isolate untrusted deserialization, preserve relevant inference and tool-use logs, and restrict model-serving identities. Keep a clean baseline so unexpected changes can be detected.

## What I took from this room

AI forensics still depends on disciplined evidence handling. The AI-specific part changes which artefacts need inspection, but the core method remains the same: preserve, identify, correlate, verify, and document.

## Phase 5: Key Takeaways & Remediation

The main goal is to connect every conclusion to observable evidence and enforce security controls at the correct trust boundary. Keep the original artefacts, record the exact commands and outputs from your own run, and redact any flag in public notes as `flag{hidden}`.
