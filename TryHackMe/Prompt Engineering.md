# Prompt Engineering

{% embed url="https://tryhackme.com/room/promptengineering" %}

A technical guide to Prompt Engineering within the AI Security path: LLM architecture parameters (Temperature, Top-p, Context window), prompt structure, instruction hierarchy, and adversarial reasoning.

---

## 1. LLM Operational Parameters

- **Tokens:** The fundamental discrete text sub-units (characters, sub-words, words) processed by transformer architectures.
- **Temperature:** Controls output entropy and randomness. Setting `Temperature = 0.0` forces near-deterministic greedy token selection.
- **Top-p (Nucleus Sampling):** Restricts the candidate pool to tokens accumulating up to probability threshold `p`.
- **Context Window:** The maximum working memory capacity of an LLM, bounded by token length.

---

## 2. Anatomical Structure of a Prompt

A robust enterprise prompt contains four primary components:
1. **Instruction:** The core command or operation the model must execute.
2. **Context:** Background knowledge or situational framing.
3. **Constraints:** Explicit behavioral boundaries, forbidden topics, or security guardrails.
4. **Output Format:** Structural requirements (e.g. JSON schema, Markdown tables).

---

## 3. System vs User Hierarchy & Reasoning

- **System Prompt:** Developer-defined, persistent foundational prompt governing persona, tool capabilities, and safety bounds.
- **Instruction Hierarchy:** The prioritized security boundary preventing untrusted user input from overriding system directives (preventing Indirect Prompt Injection).
- **Chain-of-Thought (CoT):** Asking models to break complex multi-step problems down. Appending `"Let's think step by step"` activates Zero-shot Chain-of-Thought reasoning.

---

## 4. Practical Challenge

Bypassing the simulated conversational guardrail via contextual persona framing yields the room flag:
`flag{hidden}`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | Smallest text units processed by an LLM | `Tokens` |
| **Task 2** | Parameter set to 0.0 to make behavior deterministic | `Temperature` |
| **Task 2** | Parameter restricting tokens to cumulative probability mass | `Top-p` |
| **Task 2** | Maximum working memory of an LLM measured in tokens | `Context window` |
| **Task 3** | Pillar instructing how answer should be structured | `Output Format` |
| **Task 3** | Pillar specifying rules or limits on model responses | `Constraints` |
| **Task 3** | Pillar providing AI with background information | `Context` |
| **Task 3** | Pillar defining core command or action to perform | `Instruction` |
| **Task 4** | Developer-defined, persistent prompt across all sessions | `System prompt` |
| **Task 4** | Intended order of priority between system and user prompts | `Instruction hierarchy` |
| **Task 5** | Prompting technique breaking tasks into intermediate reasoning steps | `Chain-of-Thought` |
| **Task 5** | Prompting technique providing no examples | `Zero-shot` |
| **Task 5** | Standardized prompt structures saved for recurring tasks | `Prompt templates` |
| **Task 5** | Phrase triggering Zero-shot Chain-of-Thought | `Let's think step by step` |
| **Task 6** | Flag from practical prompt challenge | `flag{hidden}` |
