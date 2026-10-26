# Python Core Concepts

Official room: https://tryhackme.com/room/pythonbasics

## Room Overview & Information

**Category:** Programming fundamentals  
**Difficulty:** Beginner  
**Focus:** Python syntax, variables, data types, collections, conditions, loops, functions, file handling, and debugging.

Python is useful in cybersecurity because it can automate log parsing, indicator validation, data transformation, and HTTP requests. A script makes work more repeatable, but it still needs tests and must not silently make assumptions about input data.

This is a programming room, not a boot-to-root target. Reconnaissance, initial access, and privilege escalation are not applicable unless the live task adds a specific authorized lab.

## Phase 1: Reconnaissance & Environment Setup

Check the Python version and start the interpreter:

```bash
python3 --version
python3
```

The first command displays the installed version; the second opens the interactive interpreter. Exit with exit() or Ctrl+D.

Save code in a file with the .py extension and run it using:

```bash
python3 script.py
```

Indentation is part of Python syntax. Code inside a function, loop, or conditional must be indented consistently. Read the complete traceback when an error occurs rather than changing unrelated lines at random.

## Phase 2: Core Syntax and Input Validation

### 1. Variables and data types

```python
username = "analyst"
attempts = 3
success = False
score = 8.5

print(type(username))
print(type(attempts))
```

A variable name refers to a value. Strings hold text, integers hold whole numbers, floats hold decimal values, and booleans represent True or False. Convert input deliberately: converting untrusted text to an integer can raise ValueError when the input is not numeric.

### 2. Lists and dictionaries

A list stores an ordered collection:

```python
ports = [22, 80, 443]
print(ports[0])
ports.append(8080)
```

Indexes begin at zero, so the item at index 0 is the first element. append() adds an item to the end.

A dictionary stores key-value pairs:

```python
event = {"user": "alice", "status": "failed"}
print(event["user"])
print(event.get("source_ip"))
```

Dictionaries are useful for structured log records. Use get() when a field may be absent rather than assuming every record contains it.

### 3. Conditions

```python
status_code = 401

if status_code == 200:
    print("Request succeeded")
elif status_code == 401:
    print("Authentication required")
else:
    print("Other response")
```

if checks the first condition, elif checks another condition if earlier conditions were false, and else handles remaining cases. Double equals compares values; a single equals assigns a value. Security scripts should distinguish known cases without assuming every unexpected value is safe.

### 4. Loops

```python
failed_users = ["alice", "bob", "carol"]

for user in failed_users:
    print(user)
```

A for loop processes each item. A while loop repeats while its condition remains true. Ensure a while loop can terminate, especially when retrying a network request or reading input.

### 5. Functions

```python
def is_failed_login(event):
    return event.get("status") == "failed"

sample = {"user": "alice", "status": "failed"}
print(is_failed_login(sample))
```

A function groups reusable logic. Parameters provide input and return sends a result back to the caller. Small functions are easier to test than one long script.

## Phase 3: File Handling, Debugging & Security Use

### 6. Read a file safely

```python
with open("events.txt", "r", encoding="utf-8") as handle:
    for line in handle:
        line = line.strip()
        if "failed" in line.lower():
            print(line)
```

The with block closes the file even if an error occurs. strip() removes surrounding whitespace, and lower() makes a simple case-insensitive check. Structured logs should be parsed using their actual format rather than searched only as raw text.

### 7. Handle expected errors

```python
try:
    attempts = int("three")
except ValueError:
    print("The input is not an integer")
```

Do not catch every exception and ignore it. Broad exception handling can hide bugs or cause a security script to report false success. Print a useful message and stop safely when required input is invalid.

### 8. Build a small security-oriented example

```python
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
```

This checks that the file exists, tolerates decoding problems by replacing invalid characters, and counts matching lines. It is a learning example, not a full log parser or reliable incident detector.

## Phase 4: Room Questions & Task Walkthrough

For each task, identify whether it tests syntax, a data type, a collection, a condition, a loop, a function, file handling, or exception behaviour. Run the example in the room's interpreter and inspect the result. If the task asks for output, use the actual interpreter output rather than copying a value from another environment.

Check typical failure points:
- A wrong indentation level changes the program structure.
- A string containing digits is not the same as an integer.
- List indexes start at zero.
- A dictionary key may be absent.
- A loop may run forever if its exit condition never becomes true.
- An unhandled exception can stop a script before it finishes.
