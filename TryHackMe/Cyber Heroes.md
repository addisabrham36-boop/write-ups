# Cyber Heroes: TryHackMe Walkthrough

Room: https://tryhackme.com/room/cyberheroes

Cyber Heroes is a themed challenge, so I should solve each task from the artefacts and clues actually supplied rather than assume the title tells me the answer. The reliable method is to identify the data type, choose a suitable tool, and verify the result before moving to the next clue.

## 1. Set up and inspect the evidence

Start the room and read the full task prompt. Note whether it provides a file, a webpage, a message, a code fragment, or an encoded string. Save any downloadable artefact in the lab directory and inspect it without executing unknown content.

For a file, basic first checks are:

    file sample
    sha256sum sample
    strings -n 5 sample | head -n 40

Replace sample with the actual filename. These commands identify the file type, calculate a hash, and print some readable strings. They do not prove a file is safe or malicious; they are initial triage.

## 2. Follow the evidence type

If the clue is a text string, inspect its character set and structure before decoding it. Base64 commonly uses a restricted alphabet and may include padding, while hexadecimal uses characters 0-9 and a-f. Neither format is encryption by itself.

If the clue is an image, inspect visible details and metadata. Metadata may be missing or misleading, so compare it with the image itself. If the clue is a web page, examine links, page source, request paths, and response behaviour within the lab.

If the task provides a network capture, identify the protocol and endpoints first, then filter the traffic to the relevant conversation. Avoid assuming a packet contains a secret until you have decoded the correct protocol layer.

## 3. Break puzzles into small steps

Write down each clue and the transformation applied to it. If a value appears encoded, decode one layer and inspect the result before trying another. If a result looks random, do not keep applying every decoder available; revisit the evidence and determine which format is plausible.

A simple evidence table can help:

| Item | Observation | Interpretation | Confidence |
|---|---|---|---|
| File or clue | What is directly visible | What it may mean | Low, medium, or high |
| Follow-up | What the next tool revealed | How it connects to the clue | Based on evidence |

This keeps a guess from quietly becoming a claimed fact.

## 4. Use tools safely

Use the AttackBox or the environment provided by TryHackMe. Do not execute unknown scripts on your personal system. Keep all network testing within the assigned target and do not upload private or sensitive artefacts to public services.

If a tool is missing, check the lab instructions and use an available equivalent. Do not install random scripts from untrusted sources just to reproduce a command.

## 5. Verify each answer

When a task asks for a specific value, trace it back to the artefact or command output that produced it. Check spelling, case, punctuation, and whether the platform expects a phrase, number, filename, or flag. The answer field may require an exact string.

This public write-up does not invent the room's output or expose a flag that has not been verified. When completing the live room, submit the exact value shown by the evidence and keep the public note focused on the method.

## What I took from this room

Themed CTFs reward disciplined evidence handling. Identify the clue type, use a tool that fits it, follow the output logically, and verify the final value instead of guessing from the story.