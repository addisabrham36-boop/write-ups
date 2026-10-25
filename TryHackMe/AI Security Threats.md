# AI Security Threats: TryHackMe Walkthrough

Room: https://tryhackme.com/room/aisecuritythreats

This room examines security problems introduced by AI and ways AI can amplify older attack methods. I treat the model as one part of a larger application: the data it can read, the tools it can invoke, the identity it runs as, and the controls around it all affect the real risk.

## 1. Recognise model-specific vulnerabilities

The main categories to recognise are:

- **Prompt injection:** untrusted text attempts to influence the model's instructions or behaviour.
- **Training-data poisoning:** manipulated examples influence the model during training.
- **Sensitive information disclosure:** the system reveals private prompts, records, or other protected data.
- **Model theft or extraction:** someone obtains model weights or tries to reproduce the model through repeated queries.
- **Adversarial input:** deliberately crafted input causes an unexpected prediction or response.
- **Model supply-chain compromise:** a model, dataset, dependency, or training component is tampered with.
- **Excessive agency:** the application gives an AI component more authority or action capability than its task requires.

When the room asks about a scenario, identify what the attacker is trying to change or obtain. Manipulating instructions is prompt injection; corrupting the learning material is poisoning; obtaining confidential information is disclosure; and using a tool beyond its intended scope is an agency or authorisation problem.

## 2. AI-enhanced attacks

AI can make existing attacks faster and more convincing. It can help produce fluent phishing messages, translate and personalise lures, generate voice or image deepfakes, and assist an attacker in writing or adapting code. The important point is that AI often improves scale and believability rather than creating an entirely new attack category.

For the room's inbox or agent exercise, read each message and classify the actual technique shown:

1. Look for an email or message designed to trick a recipient into clicking, signing in, or opening an attachment. That is phishing, even if AI helped write it.
2. Look for a synthetic or manipulated voice or image presented as a real person. That points to a deepfake.
3. Look for a tailored conversation that uses personal details or impersonation to persuade a victim to do something. That is AI-assisted social engineering.
4. Explain what AI contributed: scale, personalisation, realistic language, synthetic media, or rapid variation.

Do not label a message as a deepfake simply because it sounds unusual. The evidence in the scenario should support the classification.

## 3. Defensive uses of AI

Defenders can use AI to summarise alerts, cluster related events, extract indicators, help triage suspicious files, and support threat hunting. Those uses can save time, but the result still needs validation. A false positive can waste an analyst's time, while a false negative can hide a real incident.

A safe workflow is to preserve the original event, use AI to assist with triage, verify important claims against the underlying evidence, and record which conclusions were confirmed by an analyst.

## 4. Answering the room's questions

The room includes direct knowledge checks and an interactive scenario. For direct questions:

- If asked what technology creates convincing copies of a person's voice or appearance, think of **deepfake** generation.
- If asked which initial-access method is made harder to recognise by fluent, targeted content, think of **phishing**.
- If an agent presents example messages, classify each message and explain how AI is used rather than replying with a label alone.

The exact interactive conversation may vary. Follow the current agent instructions, provide the classification and rationale, and use the room's own feedback to verify the answer. I have not inserted a flag because I have not independently observed the current lab's flag.

## 5. Controls that reduce AI risk

- Apply least privilege to model accounts, tools, and retrieval sources.
- Enforce authorisation in application code before sensitive data is retrieved or an action is executed.
- Treat user text, webpages, documents, and retrieved records as untrusted input.
- Require human approval for financial, administrative, destructive, or otherwise high-impact actions.
- Keep model, dataset, and dependency provenance records.
- Monitor unusual tool calls, high-volume requests, data access, and repeated attempts to override instructions.
- Evaluate models with adversarial tests and realistic misuse scenarios.
- Train users to verify unusual payment, credential, or access requests through a separate trusted channel.

A system prompt is not an access-control system. Even if the model is instructed not to reveal a secret, the application must ensure it never receives data it is not authorised to disclose.

## 6. How I would document a finding

For each finding, write down the affected asset, the attack path, the evidence observed, the likely impact, and the mitigation. Separate what the room demonstrated from what I infer. Avoid copying personal information, real credentials, or private records into public notes.

## What I took from this room

AI changes the speed and realism of attacks, and it creates new risks around data, models, and tool use. The response is layered security: protect the data and model, constrain the application around it, monitor activity, and verify AI-assisted decisions rather than trusting them blindly.