# LLM Security — TryHackMe

Room: https://tryhackme.com/room/llmsecurity

Large language models can understand and generate text, but their outputs are probabilistic and can be influenced by the context they receive. When an LLM is connected to tools, private data, or external services, its security depends on more than the model alone.

## Prompt injection

Prompt injection occurs when untrusted content attempts to manipulate the model's instructions or behaviour. It may arrive directly from a user or indirectly through a webpage, document, or retrieved record. An application should not assume that the model will always ignore hostile instructions.

## Retrieval and tool use

A retrieval-augmented system can search documents and provide the results as context. Access controls must be enforced before private documents are retrieved, not merely by asking the model not to reveal them. Tools should validate their own arguments and permissions instead of trusting the model's decision.

## Reducing risk

I would limit tool permissions, isolate sensitive operations, validate outputs, monitor activity, and require approval for consequential actions. Testing should include hostile inputs and unexpected combinations, not just the normal prompts used in demonstrations.

## What I took from this room

LLM security is about controlling the boundary between generated text and real actions or sensitive information. The model can help interpret a request, but the application must enforce the actual security rules.