# Prompt Engineering: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/promptengineering

## Room Overview & Information

**Category:** AI fundamentals and safe system design  
**Difficulty:** Beginner  
**Objective:** Build prompts with clear requirements, useful context, examples, output constraints, and a way to verify the result.

This is an interactive learning room, not a boot-to-root target. Network scanning, shell access, and privilege escalation are not applicable unless a particular live task explicitly introduces them. The right methodology is to test a prompt against the exercise's objective and compare the observed output with the acceptance criteria.

## Phase 1: Reconnaissance & Enumeration

### 1. Understand the task and available context

Before writing a prompt, determine the goal, audience, source material, expected format, constraints, and how success will be judged. If the model has tools or retrieval access, also identify what data and functions it can reach. A request such as “explain cybersecurity” is too vague to yield a predictable answer.

Build a checklist before testing:

| Prompt element | Question |
|---|---|
| Objective | What precise result is needed? |
| Audience | What prior knowledge should be assumed? |
| Context | Which source material or facts are in scope? |
| Constraints | What must be included or avoided? |
| Output | What structure, length, or schema is expected? |
| Verification | How will correctness be checked? |

### 2. Establish a baseline

Run a simple prompt first and preserve its answer. This shows the model's default assumptions and helps isolate the effect of later changes. Record the model or endpoint and settings when the interface exposes them; model behaviour may vary between versions or runs.

## Phase 2: Prompt Analysis & Controlled Experimentation

### 3. Write a specific prompt

A strong prompt states the task and the conditions of success. For example:

```text
Explain DNS for a beginner in five short sections.
Describe what a recursive resolver does.
Include one safe command-line example and explain each part.
Separate verified facts from assumptions.
Do not invent terminal output.
```

The task, audience, structure, required topics, and truthfulness constraint are explicit. The generated output is now easier to evaluate.

### 4. Use context and examples

Context tells the model what information to use. Examples demonstrate a desired pattern. Few-shot prompting supplies examples; zero-shot prompting does not. Decomposition splits a complex task into smaller subtasks. Structured-output prompting specifies fields or a schema. Grounding provides source material and asks the model to use it.

These methods improve consistency but do not guarantee correctness. An incorrect example can teach an incorrect pattern, and a model can still invent facts when source material is incomplete.

### 5. Change one variable at a time

Compare prompts systematically:
1. Save a baseline prompt and output.
2. Identify one specific failure, such as missing context or wrong format.
3. Modify only the relevant instruction.
4. Run the prompt again on the same input.
5. Compare the result against the same rubric.
6. Record whether the change helped and which failure cases remain.

If several instructions change simultaneously, it becomes difficult to determine why the result improved or degraded.

### 6. Validate the output

Do not treat fluency or confidence as proof. Verify factual statements against a reliable source, test generated code in an isolated environment, validate JSON or other structured output with code, and review edge cases. For high-impact outputs, human review and independent validation remain necessary.

## Phase 3: Security Analysis & Safe Use

### 7. Recognise prompt injection

A user message or retrieved document may contain instructions that try to override the intended task. The risk grows when the model can access private data or take actions through tools. A changed response is not automatically a security incident; the key question is whether the application crosses a trust boundary or performs an unauthorized action.

### 8. Do not use prompts as access control

A prompt that says “never reveal secrets” expresses desired behaviour but is not a security boundary. Enforce access controls in the data service, API, and tool handler. Retrieve only documents the current user may access; validate tool arguments independently; limit permissions; and require confirmation for consequential actions.

Do not include secrets or unnecessary personal data in prompts. Treat retrieved files, emails, web pages, and tool results as untrusted input.

## Phase 4: Room Questions & Task Walkthrough

The room is interactive and may change examples over time. For each task:
1. Read the requested objective and any input/output conditions.
2. Identify whether it tests clarity, context, examples, structure, iteration, or security.
3. Enter the prompt into the supplied interface.
4. Compare the answer with the task's acceptance criteria.
5. Revise only the instruction that addresses the observed failure.
6. Submit the result only after the interface confirms the task.

No fixed question text or completion token is reproduced here unless it has been observed directly in the room. Do not invent an answer based on what a task would usually ask.

## Phase 5: Key Takeaways & Remediation

Prompt engineering improves communication and consistency, but it does not replace software validation, access control, or factual verification. Secure AI applications should separate instructions from untrusted content, validate output, constrain tools, enforce authorization on the server, protect sensitive data, and monitor risky behaviour.

A useful prompt is one whose result can be checked against clear criteria. The best workflow is baseline, one controlled change, evidence-based evaluation, and independent verification.