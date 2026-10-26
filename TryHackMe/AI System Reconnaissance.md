# AI System Reconnaissance

{% embed url="https://tryhackme.com/room/aisystemrecon" %}

Network discovery, framework fingerprinting, and API enumeration of specialized Artificial Intelligence and Machine Learning infrastructure (Ollama, MLflow, Triton, vector databases).

---

## 1. Traditional vs AI Reconnaissance

Conventional penetration testing focuses on common infrastructure services (SSH, HTTP, MySQL, SMB). AI reconnaissance identifies specialized ML pipeline components that operate on non-standard ports:
- **Inference Engines:** Triton Inference Server, Ollama, TorchServe, vLLM.
- **Tracking & Registries:** MLflow, Kubeflow.
- **Vector Databases:** Qdrant, Chroma, Milvus, Weaviate.

---

## 2. Fingerprinting AI Model APIs

Querying exposed REST endpoints with `curl` to identify running models and framework versions:
```bash
# Querying inference servers for loaded models
curl -s http://MACHINE_IP:11434/api/tags    # Ollama
curl -s http://MACHINE_IP:8000/v1/models     # Triton / OpenAI compatible
curl -s http://MACHINE_IP:5000/api/2.0/mlflow/experiments/list  # MLflow
```

Exposed unauthenticated model registries leak proprietary model architectures, hyperparameters, dataset source paths, and system prompts.

---

## 🏁 Questions & Answers

| Concept | Purpose | Common Port / Endpoint |
|---|---|---|
| **Inference Server** | Hosts and serves models for live prediction queries | `/v1/models` |
| **Experiment Tracker** | Logs training parameters, weights, and metrics | MLflow (port 5000) |
| **Vector Database** | Stores high-dimensional embedding vectors for RAG | Qdrant / Chroma |
| **Detection Technique** | Probing custom API responses and headers | `curl -s /v1/models` |
