# AI Models and Data: TryHackMe Walkthrough

Room: https://tryhackme.com/room/aimodelsanddata

## Room Overview & Information

This note covers the practical workflow, evidence to collect, common interpretation errors, and defensive lessons for AI Models and Data. Use the live room as the authority for task wording and machine-specific values. The repository does not include fabricated terminal output, credentials, or flags.

## 1. Start with the task and data flow

Start the room environment and identify what it asks you to inspect: a model, training dataset, prompt, inference result, or configuration file. Write down the expected input and output before testing. This baseline helps distinguish a normal model limitation from a security problem.

AI data flows often include collection, cleaning, labelling, training, evaluation, packaging, deployment, and monitoring. A weakness at one stage can propagate to later stages. For example, poisoned training examples may affect a model long after the original dataset is gone, while a changed model file may indicate supply-chain tampering rather than data poisoning.

## 2. Inspect supplied files safely

Identify files by content, not just by extension:

```bash
file ARTIFACT
sha256sum ARTIFACT
stat ARTIFACT
strings -n 6 ARTIFACT | head -n 80
```

Replace `ARTIFACT` with a real filename from the room. Record the exact hash before analysis. Do not load unknown pickle-based models or datasets with unrestricted deserialization on a personal computer; use the lab environment and safe methods recommended by the task.

For tabular datasets, inspect column names, row counts, missing values, duplicates, label distribution, and suspiciously repeated examples. For text datasets, look for duplicated phrases, unexpected instructions, strange label associations, and data that does not match the stated collection source. Avoid printing sensitive records unnecessarily.

## 3. Understand model behaviour

A model maps inputs to outputs according to learned parameters and its surrounding code. The same model can behave differently depending on preprocessing, prompt templates, decoding settings, tool access, and post-processing. Therefore, record the full inference context where the lab makes it available.

For a simple repeatable test, preserve the input and output exactly and compare results across the baseline and the suspicious case. Change one variable at a time. If the model's output changes, ask whether the change follows from the input, configuration, data, or an external tool call.

A model error is not automatically a security vulnerability. Identify the security property at risk, such as confidentiality, integrity, availability, provenance, or control of tools.

## 4. Data quality and poisoning

Training data can be incomplete, biased, duplicated, mislabeled, or deliberately poisoned. Poisoning aims to influence the model's learned behaviour, often for a trigger or target class. Evidence can include unusual clusters, repeated trigger patterns, label inconsistencies, or a reliable difference between clean and suspicious examples.

Use the room's provided analysis method and compare the suspected records with normal records. Do not claim poisoning from a single odd example. Stronger evidence comes from a repeatable relationship between a suspicious data pattern and model behaviour, plus an explanation of why that pattern is not expected.

Mitigations include provenance tracking, dataset versioning, duplicate and outlier analysis, label review, access control over data pipelines, and evaluation on a trusted holdout set. These controls reduce risk but do not guarantee that every hidden trigger will be found.

## 5. Model provenance and integrity

A model artifact should be traceable to a known source, build process, version, and approval. Compare the artifact hash with a trusted manifest when the room supplies one. If no trusted baseline exists, a hash only identifies the file you currently have; it cannot independently establish that the file is legitimate.

Secure model delivery with authenticated sources, signed manifests, controlled registries, and a review process for model updates. Restrict who can publish models and who can deploy them. Log model versions at inference time so suspicious behaviour can be linked to a specific release.

## 6. Work through the room questions

For every question, locate the specific evidence requested. A question about a data property should be answered from the dataset; a question about model behaviour should be supported by an inference test; and a question about integrity should be answered from the version or hash evidence. Preserve exact spelling and formatting for submitted values.

Where the lab uses a model or dataset that is only available inside the running environment, obtain the answer there. Do not invent row counts, hashes, output labels, or flags. Redact public flags as `flag{hidden}`.

## 7. Defensive checklist

- Track data sources, licences, owners, and versions.
- Review data quality and labels before training.
- Separate training, evaluation, and production data.
- Protect model registries and build credentials.
- Verify artifact integrity before deployment.
- Test for unexpected behaviour using representative and adversarial inputs.
- Log the model version and relevant inference metadata.
- Have a rollback process for a suspicious release.

## What I took from this room

Model security is not only about the model architecture. Data provenance, labels, preprocessing, packaging, deployment, and monitoring all affect the final behaviour. A good investigation connects the observed result to the stage that could have caused it.
