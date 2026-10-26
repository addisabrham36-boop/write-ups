# Cypheron

{% embed url="https://tryhackme.com/room/cypheron" %}

Insane-difficulty AI Supply Chain Security challenge from the *"2026: An AI Odyssey"* series: reverse-engineering a trojaned PyTorch neural model (`signal_classifier.pt`), extracting embedded neural C2 beacon tensors, and exploiting n8n pipeline path traversal to RCE.

---

## 1. Challenge Architecture & Overview

- **Difficulty:** Insane
- **Category:** AI Security / Machine Learning Supply Chain Compromise
- **Premise:** Fleet monitoring nodes report anomalous outbound traffic. A poisoned PyTorch weight distribution pipeline was pushed through a compromised vendor update mechanism.

---

## 2. Reverse Engineering the Trojaned Model

1. Intercept the PyTorch artifact `signal_classifier.pt`.
2. Inspect the tensor state dictionary with Python:
   ```python
   import torch
   model = torch.load('signal_classifier.pt', map_location='cpu')
   print(model.keys())
   ```
3. A hidden tensor calibration layer contains encoded C2 beaconing logic and embedded network strings.
4. Deserialization triggers command execution through weaponized tensor metadata.

---

## 3. Pipeline Exploitation (n8n Path Traversal to RCE)

1. The underlying workflow automation platform uses **n8n**.
2. Exploiting body parser path traversal allows reading internal environment variables and session tokens.
3. Forging administrative session tokens achieves command injection within the container.
4. Escaping the container environment via kernel-level capability abuse unlocks the root flag.

---

## 🏁 Questions & Answers

| Challenge Component | Target / Vector | Methodology |
|---|---|---|
| **AI Model Trojan** | `signal_classifier.pt` | Extracted neural C2 beacon from serialized tensor weights |
| **Pipeline RCE** | n8n workflow engine | Path traversal body parser exploit to container shell |
| **Privilege Escalation** | Host kernel escape | Container breakout to host `/root/root.txt` |
