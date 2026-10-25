# AI Models and Data: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/aimodelsanddata

## Room Overview & Information

**Category:** AI security, machine learning, and data integrity  
**Objective:** Understand the relationship between model artifacts, training and evaluation data, inference behaviour, provenance, and security controls.

This is a conceptual and artifact-analysis room rather than a traditional boot-to-root host. Port scanning, shell access, and privilege escalation are not applicable unless the active task explicitly introduces them. Use the room's data and interface as the authority for exact task-specific values.

## Phase 1: Reconnaissance & Artifact Enumeration

### 1. Map the model lifecycle

An AI system can involve data collection, cleaning, labelling, training, evaluation, packaging, deployment, inference, and monitoring. A weakness at one stage can affect later stages. Poisoned training examples may influence behaviour long after the raw dataset is removed; a tampered model file may indicate a release-process compromise.

Record which artifacts the room provides: datasets, model weights, tokenizer files, configuration, prompts, output examples, and logs. Identify what each should contain and how its source is documented.

### 2. Inspect files safely

```bash
file ARTIFACT
sha256sum ARTIFACT
stat ARTIFACT
strings -n 6 ARTIFACT | head -n 80
```

Replace ARTIFACT with a file from the room. The commands identify format, calculate a hash, record metadata, and extract a bounded sample of strings. Use the isolated lab for untrusted serialized models. Do not load unknown pickle-based model artifacts with unrestricted deserialization.

## Phase 2: Model and Data Vulnerability Analysis

### 3. Check data quality

For tabular datasets, inspect column names, record counts, missing values, duplicates, and label distribution. For text datasets, investigate repeated phrases, unusual instructions, suspicious label associations, or records inconsistent with the described source. Avoid printing sensitive records into public notes.

A suspicious example is a lead, not proof of poisoning. Stronger evidence is a repeatable relationship between a data pattern and model behaviour, supported by provenance or a trusted comparison dataset.

### 4. Understand model behaviour

A model maps input to output through learned parameters and a surrounding application. Results can change with preprocessing, prompt templates, retrieval context, tool access, decoding settings, and model version. Record those variables if they are available.

A surprising output does not automatically prove compromise. It may reflect missing context, an ordinary model limitation, prompt injection, unsafe retrieval, or a malicious artifact. Define the security property at risk, such as confidentiality, integrity, availability, provenance, or control over a tool.

### 5. Verify model provenance

Compare artifact hashes with a trusted manifest when the room supplies one. A hash identifies exact bytes, but without a trusted reference it cannot establish that a file is legitimate. Signed manifests, authenticated registries, controlled build pipelines, and release approvals provide stronger provenance.

Record the model or dataset version at inference time so a suspicious result can be linked to a particular release.

## Phase 3: Validate Findings & Post-Investigation Handling

Use repeatable comparisons to test suspected data poisoning or model backdoors. Save the baseline input/output pair, apply one controlled change, and compare results. Keep training, evaluation, and production data separate. Do not use a production model or real private data for exploratory tests without authorization.

This room does not inherently involve gaining host privileges. If a task provides shell access, verify the identity and context before claiming access. Otherwise, document the model and data finding without inventing a system compromise.

## Phase 4: Room Questions & Evidence Walkthrough

For each question, determine which evidence class it asks about:
- A data-property question should be answered from the actual dataset.
- A model-behaviour question should be supported by a reproducible inference test.
- An integrity question should be answered using an artifact version, hash, or authenticated manifest.
- A provenance question should identify the source and release process.
- A security-impact question should explain the affected property and the evidence for impact.

Preserve exact names and hash characters. Obtain machine-specific values from the active room; do not invent row counts, labels, hashes, or flags.

## Phase 5: Key Takeaways & Remediation

- Track dataset provenance, licensing, versions, and owners.
- Review labels, duplicates, outliers, and suspicious patterns before training.
- Keep evaluation data separate from training data.
- Protect model registries, build credentials, and release approvals.
- Verify artifact integrity before deployment.
- Test unexpected behaviour using representative and adversarial inputs.
- Log model versions and relevant inference metadata.
- Maintain a rollback path for a suspicious release.

Model security is not only about architecture. Data provenance, preprocessing, packaging, deployment, and monitoring all affect the final behaviour.