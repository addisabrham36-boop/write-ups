# AI Threat Modelling

{% embed url="https://tryhackme.com/room/aithreatmodelling" %}

Adapting threat modeling methodologies (STRIDE, OWASP LLM Top 10, MITRE ATLAS) to Artificial Intelligence and Retrieval-Augmented Generation (RAG) architectures.

---

## 1. AI-Specific Asset Classification

Traditional threat models evaluate databases, servers, and networks. AI threat modeling incorporates specialized machine learning assets:
- **Embedding Vectors:** Dense mathematical representations stored in vector databases (e.g. Pinecone, Chroma, Milvus) used by RAG architectures to retrieve context at query time.
- **Model Registry:** The central repository storing trained model weights, parameters, and deployment pipelines. Compromising the registry allows adversaries to swap authorized models for trojanized variants.
- **LLM Agent:** The execution layer connecting the model to APIs and tools, most vulnerable to prompt injection.

---

## 2. Threat Vector Evaluation

- **Direct Prompt Injection:** A user prompt instructing the LLM to ignore developer instructions and reveal other users' confidential balances exposes the **LLM Agent**.
- **Sensitive Information Disclosure:** Unsanitized RAG retrieval querying confidential records and returning them in model completions.

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 1** | Asset type used to retrieve relevant context in RAG systems | `Embedding Vectors` |
| **Task 1** | Asset compromised when attacker swaps a production model | `Model Registry` |
| **Task 2** | Component most exposed when user sends prompt injection message | `LLM Agent` |
| **Task 2** | Vulnerability type when system returns internal financial records | `Sensitive Information Disclosure` |
