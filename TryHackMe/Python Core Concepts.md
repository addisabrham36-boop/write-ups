# Python Core Concepts: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/pythonbasics

## Room Overview & Information

**Category:** Programming fundamentals  
**Difficulty:** Beginner  
**Focus:** Python syntax, variables, data types, collections, conditionals, loops, functions, file handling, and using scripts for security work.

Python is useful in cybersecurity because it can automate repetitive tasks such as parsing logs, validating indicators, transforming data, and making HTTP requests. A script should make the work more repeatable, but it must still be tested and should not silently make assumptions about the data.

## Phase 1: Run Python

Check the installed version:

\`\`\`bash
python3 --version
python3
\`\`\`

The first command prints the version. The second starts the interactive interpreter. Exit the interpreter with `exit()` or Ctrl+D.

A Python file commonly ends in `.py`. Run it with:

\`\`\`bash
python3 script.py
\`\`\`

Indentation is part of Python's syntax. Code inside a function, loop, or conditional must be indented consistently.

## Phase 2: Variables and data types

\`\`\`python
username = "analyst"
attempts = 3
success = False
score = 8.5
\`\`\`

A variable name refers to a value. Strings hold text, integers hold whole numbers, floats hold decimal values, and booleans represent True or False.

Check a type with `type(value)`. Convert values deliberately: converting untrusted text to an integer can raise a ValueError if it is not numeric.

## Phase 3: Lists and dictionaries

A list stores an ordered collection:

\`\`\`python
ports = [22, 80, 443]
print(ports[0])
ports.append(8080)
\`\`\`

Indexes begin at zero, so `ports[0]` is the first item. Appending adds an item to the end.

A dictionary stores key-value pairs:

\`\`\`python
event = {"user": "alice", "status": "failed"}
print(event["user"])
\`\`\`

Dictionaries are useful for structured log records. Use `event.get("source_ip")` when a field may be absent, rather than assuming every record contains it.

## Phase 4: Conditions

\`\`\`python
status_code = 401

if status_code == 200:
    print("Request succeeded")
elif status_code == 401:
    print("Authentication required")
else:
    print("Other response")
\`\`\`

- `if` checks the first condition.
- `elif` checks another condition if earlier conditions were false.
- `else` handles the remaining cases.
- `==` compares values; `=` assigns a value.

For security scripts, conditions should distinguish known cases without treating every unexpected value as safe.

## Phase 5: Loops

\`\`\`python
failed_users = ["alice", "bob", "carol"]

for user in failed_users:
    print(user)
\`\`\`

A `for` loop processes each item. A `while` loop repeats while a condition remains true. Ensure while loops can terminate, especially when reading network data or retrying a request.

## Phase 6: Functions

\`\`\`python
def is_failed_login(event):
    return event.get("status") == "failed"

sample = {"user": "alice", "status": "failed"}
print(is_failed_login(sample))
\`\`\`

A function groups reusable logic. Parameters provide input and `return` gives the result back to the caller. Small functions are easier to test than one long script.

## Phase 7: File handling

\`\`\`python
with open("events.txt", "r", encoding="utf-8") as handle:
    for line in handle:
        line = line.strip()
        if "failed" in line.lower():
            print(line)
\`\`\`

The `with` block closes the file even if an error occurs. `strip()` removes surrounding whitespace, and `lower()` makes a simple case-insensitive check. This example is intentionally basic; structured logs should be parsed using their real format rather than searching raw text alone.

## Phase 8: Errors and debugging

Use exceptions to handle expected failures:

\`\`\`python
try:
    attempts = int("three")
except ValueError:
    print("The input is not an integer")
\`\`\`

Do not catch every exception and ignore it. Broad exception handling can hide bugs or cause a security script to report a false success. Print a useful error message and exit safely when the required input is invalid.

## Phase 9: A small security-oriented example

This script reads a text file and counts lines containing the word failed:

\`\`\`python
from pathlib import Path

path = Path("events.txt")
if not path.is_file():
    raise SystemExit(f"Input file not found: {path}")

count = 0
with path.open("r", encoding="utf-8", errors="replace") as handle:
    for line in handle:
        if "failed" in line.casefold():
            count += 1

print(f"Lines containing failed: {count}")
\`\`\`

It checks that the file exists, reads it safely, tolerates decoding problems by replacing invalid characters, and counts matching lines. It is a learning example, not a full log parser or a reliable incident detector.

## Working through the room

For each question, identify whether it tests syntax, a data type, a collection, a condition, a loop, or a function. Run the example in the room's interpreter and inspect the result. If the task asks for output, use the actual interpreter output rather than copying a value from another environment.

## Key takeaways

Python becomes useful when the basics are clear: values, conditions, loops, functions, files, and exceptions. In security work, scripts should validate inputs, handle errors explicitly, and produce results that can be checked against the original evidence.

## Expanded Python practical guide

### 1. Run code and inspect errors

Use the Python interpreter or the environment supplied by the room. Save scripts with a `.py` extension and run them with the intended Python version:

```bash
python --version
python script.py
```

Read the full traceback when an error occurs. The final line identifies the exception type and message; the preceding lines show the call path. Fix the first meaningful error rather than changing unrelated lines.

### 2. Variables and types

Python variables refer to objects. Common built-in types include integers, floats, strings, booleans, lists, tuples, dictionaries, and sets. Use `type(value)` when unsure of a value's type. A string such as `"5"` is not the same as integer `5`; convert deliberately with `int()` or `str()` where appropriate.

### 3. Conditions and loops

Conditions use `if`, `elif`, and `else`. Loops repeat work: `for` iterates over an iterable, while `while` continues while a condition is true. Indentation defines code blocks, so inconsistent indentation changes program structure or raises an error.

### 4. Functions and scope

Functions package reusable behaviour. Parameters receive values, and `return` sends a result back to the caller. A local variable normally exists within its function's scope. Use descriptive names and keep functions small enough to test.

### 5. Collections and iteration

Lists are ordered and mutable; tuples are ordered and immutable; dictionaries map keys to values; sets store unique elements. Select the data structure that matches the task. Check whether a key exists before reading it when input is uncertain.

### 6. Files and exceptions

Use a context manager to ensure a file is closed:

```python
with open("notes.txt", "r", encoding="utf-8") as handle:
    content = handle.read()
print(content)
```

Catch specific exceptions where recovery is possible. Avoid broad exception handling that hides programming errors. Validate external input and do not execute untrusted strings as code.

### 7. Debugging and testing

Print small values, use a debugger, or write assertions to verify assumptions. Test normal input, empty input, unexpected types, and boundary values. Keep expected output separate from actual output so failures are easy to identify.

### 8. Complete the room

For each question, run the exact code or command in the room's environment and inspect the output. Preserve capitalization and punctuation when the answer is a literal string. Do not invent output from code that has not been run.
