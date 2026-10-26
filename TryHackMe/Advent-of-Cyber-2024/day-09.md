# Advent of Cyber 2024 - Day 9: GRC

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

Nine o'clock, make it stop!

## Background

Governance, Risk, and Compliance (GRC) forms the operational foundation of enterprise cybersecurity management:
- **Governance:** Ensures security initiatives align with business objectives and organizational goals.
- **Risk Management:** Identifies, assesses, and controls threats to an organization's digital and physical assets.
- **Compliance:** Ensures adherence to regulatory frameworks, industry mandates (PCI-DSS, HIPAA, GDPR, ISO 27001), and internal policies.

Risk is formally evaluated using qualitative and quantitative risk formulas:
$$\text{Risk} = \text{Likelihood} \times \text{Impact}$$
Understanding risk scoring enables security leaders to prioritize remediation and allocate resources effectively.

## Walkthrough

**Step 1 — Understand the Scenario**

Access the interactive GRC portal. The challenge presents an inventory of assets and identified vulnerabilities across the Wareville municipal IT environment.

**Step 2 — Calculate Asset Risk Scores**

Evaluate each asset according to Likelihood (1-5) and Impact (1-5):
- Public Web Server with unpatched RCE: Likelihood 5, Impact 5 $\rightarrow$ Risk = 25 (Critical).
- Internal Backup Server: Likelihood 2, Impact 4 $\rightarrow$ Risk = 8 (Medium).
- Workstation without screen lock: Likelihood 3, Impact 2 $\rightarrow$ Risk = 6 (Low).

**Step 3 — Implement Security Controls**

Map appropriate NIST CSF controls (Preventive, Detective, Corrective) to each asset to reduce overall organizational risk below acceptable thresholds.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What does GRC stand for? | `Governance, Risk, and Compliance` |
| What formula is commonly used to calculate risk? | `Likelihood * Impact` |
| What is the flag upon completing the risk assessment? | `THM{R15K_M4N4G3D}` |
