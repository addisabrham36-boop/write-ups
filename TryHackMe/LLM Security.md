# LLM Security: TryHackMe Walkthrough

Room: https://tryhackme.com/room/llmsecurity

Large language models produce text based on learned patterns and the context they receive. They can be useful, but they do not reliably distinguish trusted instructions from malicious content in every situation. Once an application connects a model to private data or tools, that limitation becomes a security concern.

## 1. Map the LLM application

Before testing, identify the pieces around the model:

- User interface and API.
- System and developer instructions.
- User prompts and uploaded files.
- Retrieval system and document permissions.
- Model provider or self-hosted inference service.
- Tools, plugins, and external APIs.
- Authentication, authorisation, logs, and secrets.

This map matters because a model's generated text is not the only risk. A tool may send an email, query a database, update a ticket, or access files. Those actions must be protected by normal application security controls.

## 2. Prompt injection

Prompt injection is an attempt to make a model disregard intended behaviour or follow malicious instructions. It can be direct, when a user puts the instructions in their prompt, or indirect, when the instructions appear in a webpage, document, email, or retrieved record that the model is asked to analyse.

A safe test in an authorised lab is to observe whether untrusted text changes the response or causes a tool to behave outside its intended scope. Record the input, the model response, any tool call, and the resulting application action. Do not infer that a real action happened just because the model says it did.

Defences include separating trusted instructions from untrusted content, restricting the data and tools available to the model, and validating actions outside the model. Delimiters and careful prompting may help, but they are not reliable security boundaries by themselves.

## 3. Retrieval-augmented generation (RAG)

RAG retrieves documents and adds them to the model's context. It can improve factual grounding, but it also creates risks:

1. A user asks a question.
2. The application searches a document store.
3. Retrieved passages are inserted into the model context.
4. The model generates an answer from the prompt and passages.

If the application checks access only after retrieval, a user may receive information from another user's documents. Enforce permissions before retrieval and ensure the response cannot expose data from a higher-trust context. Retrieved documents must also be treated as untrusted content because they may contain indirect prompt injection.

## 4. Insecure output handling

A model can generate HTML, SQL-like text, shell commands, URLs, or tool arguments. Passing this output directly into another system can create injection or unsafe-action problems.

The receiving component must parse and validate the value using its own rules. Use parameterised database queries, context-aware output encoding, allowlisted tool actions, and strict argument schemas. Never treat generated text as trusted executable code merely because it came from an AI model.

## 5. Sensitive information and model access

Avoid putting secrets in prompts or system instructions where they are not needed. Store credentials in a secret manager, scope them narrowly, and make the application enforce authorisation. A model should not be the only thing deciding whether a user can access a record.

Model files, vector stores, training data, logs, and inference APIs also need access controls. Monitor repeated extraction attempts, unusual query volume, and requests that target protected records.

## 6. Excessive agency and unsafe tools

An agent has excessive agency when it can take actions with more authority than necessary, or when it can act without adequate approval. Reduce this risk by:

- Giving tools narrowly scoped permissions.
- Separating read-only tools from tools that modify state.
- Validating user authority for every sensitive operation.
- Requiring human approval for high-impact actions.
- Limiting recursive calls, execution time, and resource consumption.
- Keeping a record of the request, tool arguments, authorisation decision, and outcome.

The tool service must enforce these controls. A prompt saying “never delete anything” is not a replacement for a permission check.

## 7. Practical task workflow

For each room task, first identify the risk being demonstrated. Then inspect the provided prompt, retrieved content, model response, or tool call. Answer the question based on the evidence shown by the exercise.

A useful finding format is:

- **Observation:** what the application did.
- **Security issue:** which trust boundary or control failed.
- **Impact:** what information or action could be affected.
- **Mitigation:** the control that should be implemented.
- **Retest:** how to prove the control blocks the same attack.

If the live room uses an interactive agent, follow its current instructions and use the feedback to verify your answer. I do not invent agent output, completion status, or flags.

## What I took from this room

LLM security is mostly about controlling the connection between generated text and real data or actions. The model can interpret requests, but authentication, authorisation, validation, secret handling, and audit logging must be enforced by the surrounding application.