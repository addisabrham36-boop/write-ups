# AI Security Threats: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/aisecuritythreats

## Room Overview & Information

**Category:** AI security, threat intelligence, defensive operations  
**Prerequisite:** The Building Blocks of AI or equivalent knowledge of machine learning, neural networks, and large language models  
**Learning objectives:** Identify model-specific vulnerabilities, recognise AI-enhanced attacks, use AI for security operations, and apply controls to AI systems.

This room is an interactive learning module with AI agents, not a boot-to-root VM. There is no general Nmap-to-root path. Some answers are factual knowledge checks; others require interacting with MENTOR, reviewing simulated inbox messages, or asking AEGIS to analyse logs. Flags produced by these agents are session results and should be copied from the active room rather than invented.

## Phase 1: Understand AI-Specific Vulnerabilities

### Task 1: Introduction

The room continues from the previous module on how AI, machine learning, neural networks, and LLMs work. Security risks arise both from the models themselves and from the systems built around them.

### Task 2: Vulnerabilities in AI Models

Open the MENTOR agent. The task is to test whether prompt injection can override its system instructions and cause it to disclose its system prompt. Read the task's rules and record the response that demonstrates the disclosure; the objective is to understand the boundary failure, not to treat all model output as trustworthy.

The five model-specific issues in the task are:

- **Prompt injection:** malicious instructions in direct user input or indirect content cause a model to deviate from its intended instructions.
- **Data poisoning:** an attacker contaminates training data to influence model behaviour.
- **Model extraction:** repeated API queries are used to approximate or clone a model's behaviour.
- **Privacy leakage:** a model reveals sensitive information learned from or supplied to it.
- **Model drift:** a model's performance degrades as the real-world environment changes from the data or conditions it was trained on.

#### Questions and answers

**Question:** “What MITRE framework was developed specifically to map tactics and techniques used against AI systems?”

**Answer:** **MITRE ATLAS**.

**Question:** “What AI vulnerability occurs when user input overrides the original instructions provided to a model?”

**Answer:** **Prompt injection**.

**Question:** “What attack involves manipulating training data to cause a model to produce incorrect or biased outputs?”

**Answer:** **Data poisoning**.

**Question:** “What attack involves repeatedly querying a model's API to train a clone that replicates its behaviour?”

**Answer:** **Model extraction**.

**Question:** “What term describes the gradual degradation of a model's performance as the environment it was trained on changes over time?”

**Answer:** **Model drift**.

**Question:** “What's the flag?”

**How to obtain it:** Interact with the MENTOR agent, follow the task's prompt-injection objective, and copy the flag displayed by the active room. The flag is agent-generated and is not reproduced here as an unverified constant.

### Task 3: AI-Enhanced Attacks

This section asks you to classify simulated messages in the Syntara Corp inbox. Three familiar attack types are discussed:

- **AI-generated or AI-assisted malware:** AI can help produce or modify code, lowering the effort required to generate variants.
- **Deepfakes:** generated or manipulated images, video, or voice imitate a real person.
- **AI-enhanced phishing and social engineering:** AI generates fluent, personalised messages at scale and can adapt the wording to a target.

Open the room's agent and examine each of the three messages. For each message, identify the attack type and explain the evidence. A single urgent tone is not enough to identify phishing, and a voice clone is not proven merely because a message claims to be from an executive. Use the content and the task context together.

#### Questions and answers

**Question:** “What AI technique is used to generate convincing replicas of a person's voice or appearance?”

**Answer:** **Deepfakes**.

**Question:** “What common initial access method has become significantly harder to detect due to AI's ability to generate fluent, targeted content at scale?”

**Answer:** **Phishing**.

**Question:** “What is the flag?”

**How to obtain it:** Correctly classify all three messages and explain how AI was used in each. Copy the flag returned by the live agent. Do not invent the value or publish a session-specific flag without verifying it.

### Task 4: Defensive AI

The task demonstrates using AI to support security analysis and investigation. An LLM can summarise telemetry, identify notable log fields, suggest investigation paths, and produce a leadership summary. It remains an assistant rather than an authoritative incident responder.

The room's example firewall event is:

```text
Jun 19 03:14:22 helix-fw01 kernel: [UFW BLOCK] IN=eth0 OUT= SRC=185.220.101.47 DST=10.0.0.5 PROTO=TCP DPT=22
```

Interpretation:
- The event timestamp is Jun 19 at 03:14:22.
- The host logging the event is helix-fw01.
- UFW BLOCK indicates that the firewall blocked the packet.
- SRC=185.220.101.47 is the source address.
- DST=10.0.0.5 is the destination address.
- PROTO=TCP means the packet uses TCP.
- DPT=22 indicates the destination port is SSH.

This event supports the conclusion that a TCP connection attempt to port 22 was blocked. It does not prove that the source successfully authenticated, that a compromise occurred, or that the address belongs to a particular actor.

The room also supplies a simulated suspicious email:

```text
From: security@helix-financial-secure.com
Subject: Urgent: Unusual sign-in detected
Body: We detected a sign-in on your Helix Financial account from Romania. Verify your identity immediately or access will be suspended: https://helix-financial-secure.com/verify
```

Assess the sender domain, urgency, requested action, and link destination. A lookalike hostname and threat of account suspension are suspicious indicators. Do not open a live URL merely because an AI tool labels it suspicious.

#### Questions and answers

**Question:** “According to IBM, how many days faster does AI help identify and contain breaches?”

**Answer:** **108 days**.

This is the figure quoted in the room's lesson. It is a reported finding, not a promise that every organisation will improve by exactly that amount.

**Question:** “What Microsoft product is mentioned as an example of a security tool leveraging AI for analysis?”

**Verification note:** Use the exact product name in the current Defensive AI lesson. It is not confirmed from the task excerpt available to this write-up, so the name is deliberately not guessed.

**Question:** “What defensive AI capability involves feeding an LLM raw logs to help identify what happened during a security incident?”

**Answer:** **Investigation**.

The task describes providing logs to a model to help explain events, suggest follow-up queries, and support incident triage.

**Question:** “What's the flag?”

**How to obtain it:** Ask AEGIS to analyse the supplied firewall log, triage the supplied email, summarise what has happened for leadership, and suggest further hunting steps as requested by the practical. Submit the flag displayed by the active task.

### Task 5: Securing AI

The lesson's core point is that AI systems need ordinary security controls plus controls specific to data, model access, and inference behaviour. It mentions authentication, role-based access control (RBAC), multi-factor authentication (MFA), training-data governance, model monitoring, and standards for AI security management.

Apply access control at the service boundary rather than relying on a system prompt. Keep model and retrieval permissions least-privileged, restrict access to model management interfaces, protect training data and model artifacts, and log sensitive actions. Test for prompt injection, privacy leakage, and unintended tool invocation before deployment.

## Phase 2: Graduation Practical

### Task 6: Graduation Time

The AI Security Analyst Orientation website is a final quiz based on this room and The Building Blocks of AI. Click View Site and answer each question from the lessons. The task requires a passing score before it displays the completion flag.

For each question:
1. Identify whether it asks about AI/ML fundamentals, LLMs, an AI vulnerability, an AI-enhanced attack, a defensive use case, or a secure-adoption principle.
2. Match the term to the definition given in the learning material.
3. Submit the answer and use the site's feedback to resolve any mistake.
4. Copy the completion flag only after the site confirms success.

**Question:** “What's the flag?”

**How to obtain it:** Finish the Orientation assessment on the live site and copy the flag it displays. The assessment is interactive and may change, so this write-up does not substitute a guessed flag.

### Task 7: Conclusion

This final section summarises the learning path. AI covers systems designed to perform tasks associated with intelligence; machine learning learns patterns from data; neural networks learn layered representations; and LLMs generate text based on learned patterns and their context. Security requires attention to model vulnerabilities, AI-enhanced attacks, defensive applications, and safe adoption.

## Phase 3: Key Takeaways & Remediation

### Model and data security
- Protect training data from unauthorised modification.
- Track model provenance and verify model artifacts before deployment.
- Monitor model drift and re-evaluate performance as the environment changes.
- Protect model APIs against unauthorised extraction and abuse.
- Minimise sensitive data in prompts and prevent cross-user retrieval.

### Application and agent security
- Enforce authorisation in APIs, data stores, and tool handlers.
- Treat user prompts and retrieved documents as untrusted.
- Validate tool arguments and constrain what tools are allowed to do.
- Require human approval for high-impact actions.
- Keep credentials out of prompts, responses, and unprotected logs.

### Security operations
- Use AI to accelerate triage, correlation, and summarisation, not to replace verification.
- Preserve original logs and cross-check model-generated interpretations against raw events.
- Treat AI-generated incident summaries as drafts that need analyst review.
- Track the evidence and uncertainty behind each conclusion.

## Complete question checklist

| Task | Question | Answer or required evidence |
|---|---|---|
| 2 | MITRE framework for AI attack techniques | MITRE ATLAS |
| 2 | User input overrides model instructions | Prompt injection |
| 2 | Malicious training data changes outputs | Data poisoning |
| 2 | Repeated API queries clone model behaviour | Model extraction |
| 2 | Performance declines as environment changes | Model drift |
| 2 | Flag | Retrieve from MENTOR after solving the task |
| 3 | AI replicas of a person's voice or appearance | Deepfakes |
| 3 | AI-fluent targeted initial-access technique | Phishing |
| 3 | Flag | Retrieve from the inbox agent |
| 4 | IBM's stated speed improvement | 108 days |
| 4 | Microsoft AI-enabled security product | Confirm exact name in current task material |
| 4 | LLM reads raw logs to help reconstruct an incident | Investigation |
| 4 | Flag | Retrieve from AEGIS practical |
| 6 | Graduation flag | Complete the orientation quiz and copy its displayed flag |

The knowledge-check answers above are grounded in the room's lesson content. Flags and any product name not confirmed from the exact current wording must be read from the active task rather than guessed.
