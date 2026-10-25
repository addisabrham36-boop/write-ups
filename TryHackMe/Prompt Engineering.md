# Prompt Engineering — TryHackMe

Room: https://tryhackme.com/room/promptengineering

Prompt engineering is the practice of writing instructions and context that help a language model produce a useful response. A prompt can specify the task, audience, format, constraints, and examples. Clear instructions generally make the output easier to evaluate, although no prompt guarantees correctness.

## Give the model a clear task

A vague request such as “explain security” leaves many decisions unspecified. A better prompt identifies the topic, level of detail, and desired structure. For example, I can ask for a beginner-friendly explanation of a network concept with one safe example and a short list of limitations.

Examples can demonstrate the output format I want. Constraints can also reduce unwanted output, but they do not replace checking the result.

## Evaluate the response

Language models can generate plausible but incorrect information. I should verify factual claims, test code in a safe environment, and use trusted sources for important decisions. If a model does not know something, a confident answer is still not evidence.

## Security considerations

Prompts can also be used to manipulate a model into ignoring intended boundaries or revealing information it should not disclose. This is why applications should not rely on prompt wording as the only security control. Sensitive actions need validation, access controls, and clear separation between trusted instructions and untrusted input.

## What I took from this room

Good prompts make the task and expected result clear, but the model's output still needs review. Prompt engineering improves communication with a model; it does not turn the model into a guaranteed source of truth or a substitute for application security.