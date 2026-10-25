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