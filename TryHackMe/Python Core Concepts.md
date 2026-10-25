# Python Core Concepts — TryHackMe

Room: https://tryhackme.com/room/pythonbasics

I wanted to work through the Python basics because scripting is useful in cybersecurity for all sorts of jobs. It can help me read logs, process a list of IP addresses, automate repetitive checks, or understand a tool that is written in Python. I do not need to become a professional developer before using it, but I do need to understand what the code is actually doing instead of copying commands and hoping the computer respects my feelings.

## Variables and data types

A variable is a name I use to store a value. For example:

```python
name = "Abrham"
age = 18
is_learning = True
```

Here, `name` is a string, `age` is an integer, and `is_learning` is a Boolean. Python also has floating-point numbers for values with decimals, and collections such as lists and dictionaries for keeping related data together.

The type matters because different values support different operations. Adding two numbers gives a numeric result, while joining strings combines text. If I am unsure what type a value has, I can check it with `type(value)`.

## Conditions

Conditions let a program choose what to do based on whether something is true or false.

```python
attempts = 3

if attempts >= 3:
    print("Check the account")
else:
    print("Keep monitoring")
```

The indentation is part of Python's syntax, not decoration. The code inside the `if` block is only executed when the condition is true. I can use `elif` when I need to check another condition before falling back to `else`.

## Loops

A loop repeats work. A `for` loop is useful when I want to go through items in a collection:

```python
ports = [22, 80, 443]

for port in ports:
    print(port)
```

A `while` loop repeats while its condition remains true. I need to be careful with it, because if the condition never becomes false, the program can keep running forever. Computers are very committed to following bad instructions.

## Functions

Functions let me package a task so I can reuse it instead of writing the same logic over and over.

```python
def is_common_web_port(port):
    return port in [80, 443]

print(is_common_web_port(443))
```

The function takes a value called `port` and returns whether it is one of the listed ports. Returning a value is different from printing it: `return` gives the result back to the code that called the function.

## Working with files

Python can read text files, which is useful when working with logs or lists. A safe basic pattern is to open a file with a context manager:

```python
with open("sample.txt", "r", encoding="utf-8") as file:
    contents = file.read()

print(contents)
```

The `with` block handles closing the file when the block finishes. The file needs to exist at that path, otherwise Python raises an error. When I write scripts that handle real data, I should also consider missing files, invalid input, and whether I have permission to access the file.

## What I took from this room

The main thing is to understand the building blocks: variables store values, conditions make decisions, loops repeat tasks, functions organise reusable logic, and file handling lets a program work with data outside the script. These basics are enough to start writing small helper scripts and to make more sense of Python-based security tools.

This is a fundamentals write-up, not a record of a specific exploit or completed target. I should add exact room answers or terminal output only when I have checked them against the actual task.
