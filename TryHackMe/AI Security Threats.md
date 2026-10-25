# AI Security Threats — TryHackMe

Room: https://tryhackme.com/room/aithreats

AI systems introduce security risks in addition to the risks already present in ordinary software. The exact threats depend on how the model is trained, what data it can access, and what actions the surrounding application allows it to perform.

## Common threat categories

- **Prompt injection:** untrusted input attempts to override the application's intended instructions.
- **Data poisoning:** training or reference data is manipulated to influence behaviour.
- **Sensitive information disclosure:** the system reveals data it should not expose.
- **Model theft or extraction:** an attacker tries to reproduce a model or infer sensitive properties.
- **Adversarial input:** crafted input causes unexpected model behaviour.
- **Excessive agency:** an AI application has more permissions or action capability than it needs.

These threats are not solved by one clever system prompt. Security needs to be built around the model, data sources, tools, and permissions.

## Practical defences

Applications should validate inputs and outputs where appropriate, restrict the model's access to tools and data, separate trusted instructions from untrusted content, and require human approval for sensitive actions. Logging and testing should cover both ordinary use and adversarial cases.

## What I took from this room

AI security is a system problem, not just a model problem. Even a capable model can become risky if it receives untrusted instructions, has broad permissions, or can access sensitive data without proper checks. Defence needs layered controls and regular evaluation.