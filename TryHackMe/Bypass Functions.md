# Bypass Functions: TryHackMe Walkthrough

Room: https://tryhackme.com/room/bypassfunctions

This note treats the room as a controlled exercise in understanding application restrictions and the difference between a filter and a security boundary. The exact language, function, and restrictions must be taken from the live task; names alone are not enough to infer the intended bypass.

## 1. Read the constraints first

Start the lab and identify which input field, function, or execution environment the task provides. Record what is allowed, what is blocked, and what output the application returns. Do not begin by trying random strings. The goal is to understand the rule being enforced and whether the rule actually prevents the unsafe behaviour.

## 2. Separate filtering from security enforcement

A blacklist rejects a known set of strings or characters. It can fail when equivalent syntax, alternate encodings, or another code path reaches the same operation. An allowlist defines the specific input forms that are acceptable and is usually easier to reason about when the expected format is narrow.

Even an allowlist is not enough if the application later interprets the input in a dangerous context. A value safe as plain text may become unsafe when inserted into a shell command, SQL statement, HTML document, or template without context-aware handling.

## 3. Reproduce the behaviour methodically

For each test:
1. Record the original input and expected result.
2. Change one property at a time.
3. Observe the exact error or output.
4. Determine whether the server rejected the input, transformed it, or interpreted it.
5. Repeat only with values permitted by the lab.
6. Document the smallest input that demonstrates the issue.

This approach helps distinguish a real bypass from an unrelated error or a display quirk. It also makes the finding reproducible.

## 4. Understand execution context

The meaning of a character depends on where it is interpreted. A quote may be harmless inside a string but special in a shell, SQL query, or programming-language expression. Encoding changes representation; it does not automatically make an input safe after decoding.

Trace the data from the user-controlled field through validation and into the operation that consumes it. If the value is passed to a parser or interpreter, identify the exact grammar and the protections that apply at that boundary.

## 5. Verify impact without causing damage

Use a harmless proof in the authorised lab. Avoid destructive file operations, credential theft, persistence, or actions against systems outside the assigned environment. If the exercise demonstrates command execution, confirm it with a benign result and record the execution identity.

A filter bypass does not automatically prove full compromise. State exactly what was demonstrated and what remains unverified.

## 6. Defensive design

- Prefer structured APIs over constructing executable strings.
- Use parameterised queries for database operations.
- Apply context-aware output encoding for HTML and other interpreters.
- Validate against a strict allowlist.
- Keep dangerous functionality isolated and minimally privileged.
- Add tests for alternate encodings and equivalent syntax.
- Log rejected inputs and anomalous behaviour without unnecessarily storing secrets.

Disabling one function may reduce exposure, but it does not fix the underlying design if another function provides the same unsafe capability.

## 7. Working through the room

Use the restrictions and examples supplied by the current task. Identify the intended operation, determine how the filter works, and test one hypothesis at a time. The accepted answer may depend on the specific lab code, so verify the current output rather than copying a guessed payload from another guide.

## What I took from this room

A bypass is meaningful only when it crosses a defined security boundary. Understanding the input path and execution context leads to a stronger finding and a more durable fix than memorising a list of special characters.