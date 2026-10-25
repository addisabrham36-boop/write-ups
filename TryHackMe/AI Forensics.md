# AI Forensics — TryHackMe

Room: https://tryhackme.com/room/aiforensics

AI forensics involves investigating evidence related to AI systems, such as model interactions, prompts, outputs, data sources, and tool activity. The exact artefacts depend on the application and what it records.

## Preserve the evidence

I should record where the artefact came from, when it was collected, and whether it was changed during analysis. If a system logs prompts and outputs, those records may help reconstruct a sequence of events, but they may be incomplete or contain sensitive information.

Timestamps need context, especially when services use different time zones or clocks. I should distinguish the raw record from my interpretation and avoid filling gaps with assumptions.

## Investigating an AI incident

Useful questions include: what input was provided, what data was retrieved, which tools were called, what permissions were active, and what output or action followed? If a prompt injection is suspected, I should examine both the malicious content and the application's control boundaries. The model's final answer alone may not show the full chain of events.

## Privacy and limitations

Logs may contain personal data, credentials, or confidential documents. Access and retention should follow the applicable policy. A missing log entry does not prove an event never happened, and a generated answer does not prove that a real action was completed.

## What I took from this room

AI investigations require careful timelines, preserved artefacts, and clear separation between evidence and inference. The surrounding application logs and tool records can be just as important as the model's text output.