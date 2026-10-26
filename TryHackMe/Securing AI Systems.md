# Securing AI Systems

{% embed url="https://tryhackme.com/room/securingaisystems" %}

Architectural security principles for AI-augmented enterprise applications: prompt construction pipelines, tool integration boundaries, OWASP Top 10 for LLMs, and least-privilege orchestration.

---

## 1. Architectural Shift: Traditional vs AI-Augmented

Traditional applications rely on deterministic, structured data flows with well-defined parameter validation boundaries. AI-augmented systems introduce probabilistic, natural-language components that interpret untrusted user input as natural commands.

Key Architectural Boundaries:
- **Prompt Construction Layer:** Combines the system prompt, retrieved context (RAG), and raw user input before invoking the model API.
- **LLM-to-Tools Boundary:** The critical execution threshold crossed whenever model output triggers an external action, database query, or API call.

---

## 2. OWASP Top 10 for LLMs

- **LLM02: Sensitive Information Disclosure:** Unsanitized context retrieval leaking internal credentials or proprietary data.
- **LLM06: Excessive Agency:** Granting autonomous agent access to execute high-impact destructive operations (e.g. `DROP TABLE`, deleting files) without human-in-the-loop verification.
- **LLM07: System Prompt Leakage:** Prompt injection forcing the model to reveal its confidential initialization instructions.

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | Layer responsible for combining system prompt, user input, and retrieved context | `Prompt Construction` |
| **Task 2** | Boundary crossed when LLM output triggers a database query | `LLM-to-tools` |
| **Defensive** | Core design principle to mitigate Excessive Agency | Strict least privilege, input/output validation, and human-in-the-loop confirmation |
