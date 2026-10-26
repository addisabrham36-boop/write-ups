# Python Core Concepts

{% embed url="https://tryhackme.com/room/pythoncoreconcepts" %}

Foundations of Python programming for cybersecurity scripting: data types, type casting, string operations, lists, dictionaries, conditional branching, and loop control statements.

---

## 1. Data Types & Type Casting

Python features dynamic typing. Security scripts frequently cast input types (e.g. converting user CLI input strings into port integers):
```python
raw_port = "443"
port = int(raw_port)      # Cast string to int
valid = bool(port > 0)    # Boolean True
```

---

## 2. Core Data Structures

- **Lists:** Ordered, mutable sequences suitable for tracking IP addresses or wordlists:
  ```python
  targets = ["10.10.10.1", "10.10.10.2"]
  targets.append("10.10.10.3")
  ```
- **Dictionaries:** Key-value mappings ideal for storing service banners and configuration payloads:
  ```python
  service = {"port": 80, "name": "HTTP", "state": "open"}
  ```

---

## 3. Control Flow & Loop Control

- `break`: Terminates the nearest enclosing loop immediately.
- `continue`: Skips the remainder of the current iteration and advances to the next loop evaluation.
- `range(start, stop, step)`: Generates sequence iterators (e.g. port range scanning).

---

## 🏁 Questions & Answers

| Concept | Description | Example |
|---|---|---|
| **Type Conversion** | Converting string port input to integer | `int("80")` |
| **String Formatting** | Modern readable interpolation | `f"Target IP: {ip}"` |
| **Data Structure** | Key-value store for mapping banners | `dict` |
| **Loop Control** | Skips current loop iteration | `continue` |
| **Loop Control** | Exits loop completely | `break` |
