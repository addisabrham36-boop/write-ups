# AI Models and Data

{% embed url="https://tryhackme.com/room/aimodelsanddata" %}

A foundational exploration of machine learning data pipelines, data provenance, training epochs, overfitting vs generalisation, quantization optimizations, and federated learning architectures.

---

## 1. Training Data & Provenance

- **Data Provenance:** The verifiable historical record documenting where training data originated, when it was scraped, and whether it has undergone transformation or sanitization.
- **Common Crawl:** The massive public web crawl corpus underpinning modern large language model foundations.
- **ML-BOM (Machine Learning Bill of Materials):** The AI equivalent of an SBOM documenting dataset origins, pipeline steps, and licensing terms.

---

## 2. Model Training & Optimization

- **Epoch:** One complete training cycle passing the entire dataset through the model architecture.
- **Overfitting:** The failure mode where an ML model memorizes exact training samples rather than generalizing to unseen data.
- **Quantisation:** Post-training optimization reducing weight precision (e.g. from FP32 to INT8/INT4), slashing memory footprint and computational requirements.
- **Federated Learning:** Decentralized training where edge devices train on local data and submit only gradient/weight updates to a central server.
- **Fine-tuning:** Continued supervised training of a foundational pre-trained model on specialized domain data.
- **Model Card:** Standard documentation outlining model scope, training parameters, evaluation metrics, and safety boundaries.

---

## 3. Practical Exercise

Completing the training optimization lab produces the flag:
`flag{hidden}`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | Ability to answer where data came from and if modified | `Data Provenance` |
| **Task 2** | Public corpus underpinning essentially every major model family | `Common Crawl` |
| **Task 2** | AI equivalent of an SBOM | `ML-BOM` |
| **Task 3** | One complete pass through the entire dataset | `Epoch` |
| **Task 3** | Problem when model memorizes training data | `Overfitting` |
| **Task 3** | Technique reducing numerical precision of weights | `Quantisation` |
| **Task 3** | Decentralized training sending only weight updates | `Federated Learning` |
| **Task 4** | Continuing training a pre-trained model on domain data | `Fine-tuning` |
| **Task 4** | Model already trained on a large general dataset | `Pre-trained Model` |
| **Task 5** | Documentation describing what a model is and its limitations | `Model Card` |
| **Task 5** | Billions of floating-point numbers in a model | `Weights` |
| **Task 6** | Practical completion flag | `flag{hidden}` |
