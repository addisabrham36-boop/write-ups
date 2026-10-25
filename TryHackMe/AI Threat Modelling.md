# AI Threat Modelling — TryHackMe

Room: https://tryhackme.com/room/aithreatmodeling

Threat modelling is a structured way to identify what can go wrong in a system, how an attacker might cause it, and which controls can reduce the risk. For AI systems, the model is only one component. Data pipelines, retrieval stores, tools, users, and infrastructure also need to be considered.

## Start with the system

I would map the important components and data flows, then identify trust boundaries. For an AI assistant, those might include user input, retrieved documents, model context, tool calls, and the external services that carry out actions. Each boundary raises questions about who can provide data and what that data is allowed to influence.

## Identify threats and impact

Possible threats include prompt injection, poisoned data, sensitive information disclosure, model extraction, and tools being used beyond their intended purpose. I should consider the assets at risk and the likely impact rather than treating every theoretical issue as equally serious.

## Choose controls and test them

Controls might include least privilege, access checks before retrieval, output validation, human approval for high-impact actions, monitoring, and adversarial testing. I should test whether these controls still work when inputs are malicious or confusing.

## What I took from this room

Threat modelling turns a vague concern about AI safety into concrete scenarios and controls. The useful result is not a long list of scary words; it is a map of what needs protection, how it could fail, and how the system will prevent or detect that failure.