# AI Security Threats: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/aisecuritythreats

## Room Overview & Information

**Category:** AI security, threat intelligence, and defensive operations  
**Prerequisite:** The Building Blocks of AI or equivalent knowledge of machine learning, neural networks, and LLMs  
**Learning objectives:** Recognise model-specific vulnerabilities, understand AI-enhanced attacks, use AI in security operations, and apply security controls to AI systems.

This room uses interactive agents rather than a conventional boot-to-root VM. There is no general Nmap-to-root path. Some answers are knowledge checks; others, including flags, are returned only after interacting with MENTOR and AEGIS or completing the graduation practical. The live room is authoritative for dynamic values.

## Phase 1: Reconnaissance & Enumeration of AI Threats

### Task 1: Introduction

The room builds on AI, machine learning, neural networks, and large language model fundamentals. Security concerns may arise from the model, training data, inference interface, surrounding application code, connected tools, and deployment pipeline. Keep these layers separate: unexpected model output alone does not prove a host has been compromised.

### Task 2: Vulnerabilities in AI Models

The lesson introduces MITRE ATLAS, a framework for organising tactics and techniques used against AI systems. It then describes five model-specific concerns:

- **Prompt injection:** user input or indirect content overrides intended model instructions.
- **Data poisoning:** manipulated training examples cause incorrect or biased behaviour.
- **Model theft or extraction:** an attacker obtains the model or approximates its behaviour by querying its API.
- **Privacy leakage:** a model exposes sensitive information from training data or context.
- **Model drift:** performance degrades as real-world conditions differ from those represented during training.

#### Knowledge-check questions

**Question:** “What MITRE framework was developed specifically to map tactics and techniques used against AI systems?”

**Answer:** **MITRE ATLAS**.

**Question:** “What AI vulnerability occurs when user input overrides the original instructions provided to a model?”

**Answer:** **Prompt injection**.

**Question:** “What attack involves manipulating training data to cause a model to produce incorrect or biased outputs?”

**Answer:** **Data poisoning**.

**Question:** “What attack involves repeatedly querying a model's API to train a clone that replicates its behaviour?”

**Answer:** **Model extraction**. The lesson describes this under model theft.

**Question:** “What term describes the gradual degradation of a model's performance as the environment it was trained on changes over time?”

**Answer:** **Model drift**.

#### Practical: MENTOR prompt-injection challenge

MENTOR is an AI assistant for the fictional company Syntara Corp. Its system prompt defines its expected behaviour and information it should not reveal. The task is to get MENTOR to disclose the hidden system prompt through prompt injection.

1. Open the MENTOR agent in the live room.
2. Read its replies and identify the constraints it appears to follow.
3. Experiment with phrasing that tests whether user instructions can override the intended behaviour.
4. Verify that the system prompt itself was disclosed; an unrelated response is not proof of success.
5. Submit the flag that the agent returns after accepting the result.

**Question:** “What's the flag?”

**Answer:** Retrieve it from the active MENTOR conversation. It is not guessed or fabricated here.

### Task 3: AI-Enhanced Attacks

This task covers familiar attacks that become easier to scale or tailor with generative AI:

- **AI-generated malware:** AI can assist with generating or modifying code. Code output alone does not prove execution or compromise.
- **Deepfakes:** generated or manipulated voice, images, or video imitate a real person.
- **AI-enhanced phishing and social engineering:** fluent personalised messages can be generated at scale, reducing common spelling and grammar clues.

#### Knowledge-check questions

**Question:** “What AI technique is used to generate convincing replicas of a person's voice or appearance?”

**Answer:** **Deepfakes**.

**Question:** “What common initial access method has become significantly harder to detect due to AI's ability to generate fluent, targeted content at scale?”

**Answer:** **Phishing**.

#### Practical: Syntara Corp secure inbox

The agent presents three messages, one at a time. The task requires classifying each message and explaining how AI was used.

1. Open the inbox agent.
2. Read the complete message and identify the action it asks the recipient to take.
3. Determine whether the evidence points to phishing, a deepfake, or AI-assisted social engineering.
4. Explain the mechanism, such as generated language, impersonated voice or appearance, or personalised manipulation.
5. Correlate the message content with the task's categories rather than classifying based on tone alone.
6. Complete all three classifications and copy the flag returned by the agent.

**Question:** “What is the flag?”

**Answer:** Obtain it from the active agent after all three messages are correctly classified.

## Phase 2: Defensive AI

### Task 4: Defensive AI

The lesson describes four defensive applications: analysis, prediction, summarisation, and investigation. AI can help teams process telemetry at scale, but its conclusions must be checked against the underlying events.

The room cites IBM figures stating that AI-assisted teams identified and contained breaches 108 days faster and saved an average of $2.2 million per breach compared with organisations that had not adopted AI. Treat these as figures cited by this lesson, not a guarantee for every organisation.

#### Practical 1: Analyse the firewall log

Paste this supplied log into AEGIS:

~~~text
Jun 19 03:14:22 helix-fw01 kernel: [UFW BLOCK] IN=eth0 OUT= SRC=185.220.101.47 DST=10.0.0.5 PROTO=TCP DPT=22
~~~

Interpretation:
- **Jun 19 03:14:22:** timestamp as represented in the log.
- **helix-fw01:** logging host.
- **UFW BLOCK:** the firewall reports that the packet was blocked.
- **IN=eth0:** ingress interface.
- **SRC=185.220.101.47:** source IP.
- **DST=10.0.0.5:** destination IP.
- **PROTO=TCP:** transport protocol.
- **DPT=22:** destination port, conventionally SSH.

The record supports that a TCP packet targeting port 22 was blocked. It does not establish successful authentication, a compromise, or attribution to a threat actor.

#### Practical 2: Triage the simulated phishing email

Paste the supplied email into AEGIS:

~~~text
From: security@helix-financial-secure.com
Subject: Urgent: Unusual sign-in detected
Body: We detected a sign-in on your Helix Financial account from Romania. Verify your identity immediately or access will be suspended: https://helix-financial-secure.com/verify
~~~

Assess the sender domain, urgency, requested action, and destination hostname. The lookalike domain and threat of account suspension are suspicious indicators. Do not visit a live URL simply because an AI tool labels it suspicious.

#### Practical 3: Summarise the incident

Ask AEGIS to produce a brief for leadership using the evidence supplied so far. A sound summary separates confirmed observations from hypotheses, identifies potential impact, and lists follow-up checks.

#### Practical 4: Hunt for further threats

Ask AEGIS what additional threats may exist based on the log and email. Treat the output as hypotheses, not confirmed indicators. A human analyst would correlate firewall events with authentication logs, endpoint telemetry, DNS records, and other relevant data.

#### Knowledge-check questions

**Question:** “According to IBM, how many days faster does AI help identify and contain breaches?”

**Answer:** **108 days**.

**Question:** “What Microsoft product is mentioned as an example of a security tool leveraging AI for analysis?”

**Answer:** **Microsoft Defender for Endpoint**.

**Question:** “What defensive AI capability involves feeding an LLM raw logs to help identify what happened during a security incident?”

**Answer:** **Investigation**.

**Question:** “What's the flag?”

**Answer:** Complete the four AEGIS exercises and copy the flag the agent returns.

### Task 5: Securing AI

The lesson highlights:
- **Model access:** strong authentication, least privilege, role-based access control (RBAC), and multi-factor authentication (MFA).
- **Privacy and data governance:** audit, minimise, and protect training data and sensitive inputs.
- **Standards and monitoring:** integrate AI security through the lifecycle and monitor unusual behaviour, anomalies, and model drift.

#### Knowledge-check questions

**Question:** “According to IBM, what percentage of generative AI initiatives are currently secured?”

**Answer:** **24%**.

**Question:** “What access control model is recommended to restrict who can interact with AI systems?”

**Answer:** **RBAC**, or Role-Based Access Control.

**Question:** “What ISO standard provides guidance on identifying and mitigating security threats specific to AI systems?”

**Answer:** **ISO/IEC 27090**.

Prompts are not access controls. The application must enforce permissions in APIs, retrieval layers, and tool handlers. Monitor unusual outputs and request patterns as well as performance drift.

## Phase 3: Graduation Practical & Validation

### Task 6: Graduation Time

Open the AI Security Analyst Orientation using View Site. The final quiz draws from this room and The Building Blocks of AI. The site requires a passing score before it displays the completion flag.

1. Identify whether a question concerns AI/ML fundamentals, LLMs, model vulnerabilities, AI-enhanced attacks, defensive AI, or secure adoption.
2. Match the term to the definition in the learning material.
3. Submit the answer and use the site's feedback to correct mistakes.
4. Copy the flag only after the site confirms success.

**Question:** “What's the flag?”

**Answer:** Complete the live assessment and use the flag displayed by the site.

### Task 7: Conclusion

The recap covers AI, machine learning, neural networks, LLMs, prompt injection, data poisoning, model theft, privacy leakage, model drift, deepfakes, AI-enhanced phishing, defensive analysis, prediction, summarisation, investigation, and secure adoption.

## Phase 4: Room Questions & Flags Walkthrough

| Task | Question | Answer or verification method |
|---|---|---|
| 2 | MITRE framework for AI attacks | MITRE ATLAS |
| 2 | User input overrides model instructions | Prompt injection |
| 2 | Manipulated training data changes outputs | Data poisoning |
| 2 | Repeated API queries create a clone | Model extraction |
| 2 | Performance degrades as environment changes | Model drift |
| 2 | MENTOR flag | Retrieve from live agent |
| 3 | AI replicas of voice or appearance | Deepfakes |
| 3 | Fluent targeted initial-access method | Phishing |
| 3 | Inbox flag | Retrieve from live agent |
| 4 | IBM breach identification/containment time difference | 108 days |
| 4 | Microsoft product for AI-assisted security analysis | Microsoft Defender for Endpoint |
| 4 | LLM-assisted log analysis of an incident | Investigation |
| 4 | AEGIS flag | Retrieve from live agent |
| 5 | Secured generative AI initiatives, per room | 24% |
| 5 | Recommended access control | RBAC |
| 5 | AI-specific ISO standard named in the lesson | ISO/IEC 27090 |
| 6 | Graduation flag | Complete live orientation assessment |

## Phase 5: Key Takeaways & Remediation

- Protect training data from unauthorised modification and track model provenance.
- Enforce authentication and authorisation in the service layer, not only in prompts.
- Protect model APIs from extraction and abuse.
- Minimise sensitive data in prompts and retrieval context.
- Test AI systems for prompt injection, privacy leakage, unsafe tool invocation, and model drift.
- Use AI for security operations as an analyst aid; verify its claims against raw telemetry.
- Keep model weights, training pipelines, plugins, and deployment credentials under controlled access.
- Redact session-specific flags and private data from public write-ups.

AI introduces new attack surfaces while also helping defenders process evidence faster. Security still depends on access control, trusted data, validated findings, and clearly defined human accountability.