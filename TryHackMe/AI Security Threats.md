# AI Security Threats

{% embed url="https://tryhackme.com/room/aisecuritythreats" %}

An in-depth guide to AI and Machine Learning security risks: model inversion, data poisoning, adversarial perturbation attacks, MITRE ATLAS threat mappings, and model monitoring tools (SHAP/LIME).

---

## 1. Adversarial AI Threat Landscape

Machine learning systems introduce unique attack vectors distinct from traditional application vulnerabilities:
- **Model Inversion / Theft:** Adversaries probe models via black-box API interactions to clone weights or infer training records.
- **Data Poisoning:** Injecting adversarial samples during training or fine-tuning to create stealth backdoors.
- **MITRE ATLAS:** The industry taxonomy cataloging real-world attacks against ML-enabled applications.

---

## 2. Defensive Tooling & Monitoring

Explainability frameworks like **SHAP** (SHapley Additive exPlanations) and **LIME** (Local Interpretable Model-agnostic Explanations) provide transparency into node activation, enabling continuous **model monitoring** to detect drift, bias, and adversarial inputs.

---

## 3. Practical Exercise

Completing the adversarial parameter calculation challenge produces the flag:
`thm{443/60/16384}`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | Machine learning combining labeled and unlabeled data | `semi-supervised learning` |
| **Task 2** | First layer in a neural network handling raw data | `input layer` |
| **Task 2** | Learning method extracting features from raw unstructured input | `deep learning` |
| **Task 2** | Weighted connections between nodes in a neural network | `synapses` |
| **Task 3** | AI model enabling ChatGPT advancements | `large language models` |
| **Task 3** | Initial training stage processing massive data | `pre-training` |
| **Task 3** | Neural network architecture introduced by Google in 2017 | `transformer` |
| **Task 4** | MITRE framework cataloging AI cyber threats | `ATLAS` |
| **Task 4** | Attack involving cloning a model by querying its API | `model theft` |
| **Task 4** | Generative AI technique replicating voice or appearance | `deepfake` |
| **Task 4** | Common attack made harder to detect due to fluent AI text | `phishing` |
| **Task 5** | Days faster AI helps identify and contain breaches (IBM) | `108` |
| **Task 5** | Cybersecurity task benefiting from AI to imagine adversary behaviors | `threat hunting` |
| **Task 5** | Explainability tools like SHAP and LIME assist with | `model monitoring` |
| **Task 6** | Flag from practical exercise | `thm{443/60/16384}` |
