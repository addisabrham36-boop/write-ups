# Advanced SQL Injection: Advent of Cyber 2024

Room: https://tryhackme.com/room/adventofcyber2024

This challenge focuses on SQL injection in an Advent of Cyber scenario. SQL injection occurs when untrusted input changes the structure of a database query rather than being treated as data. The exact payload depends on the query context, database engine, and how the application handles errors, so the task's evidence matters more than memorising a string.

## 1. Understand the query context

A web application may build a query using values from a login form, search field, product identifier, or filter. If it concatenates user input into SQL, special syntax may change the query's meaning. The first step is to identify the affected input and compare normal behaviour with the lab's expected test.

Record the request method, endpoint, parameter name, response code, and response body. Change one input property at a time. A changed error message can be a clue, but it does not by itself prove that arbitrary SQL executed.

## 2. Determine how the application responds

SQL injection behaviour varies. Some applications reveal database errors, while others return generic messages. A Boolean-based test compares responses to conditions that should evaluate differently. A time-based test looks for a controlled delay. Union-based techniques depend on the original query's structure and compatible result columns.

Use only the method and target described by the active task. Do not assume that a payload from a different database or endpoint will work. If the room provides a request in Burp Suite, keep the other fields constant and compare responses carefully.

## 3. Follow the task's evidence

For each question, identify what the task asks you to discover: whether input affects a query, how many columns are returned, which database is in use, or what data the exercise exposes. Use the supplied lab and record the actual response. Avoid inventing table names, schema details, query output, or flags.

If the task requires extracting data, remain within the authorised lab and access only what is needed to complete the exercise. Do not apply the same payload to unrelated websites.

## 4. Common mistakes

- Assuming every database uses the same syntax.
- Mixing quote types or comments without understanding the query.
- Changing several request fields at once and losing track of the cause.
- Treating a generic server error as proof of SQL injection.
- Forgetting that the application may decode or normalise input before processing it.
- Assuming a successful response means the expected database operation occurred.

## 5. Defensive lesson

The primary defence is to use parameterised queries or prepared statements so that user-supplied values remain data. Avoid building SQL by concatenating strings. Apply least privilege to the database account, limit sensitive error details shown to users, and log suspicious input safely.

Input filtering alone is fragile because SQL syntax and encoding vary. A regression test should verify that a malicious-looking value is treated as a literal value rather than changing query structure.

## 6. Working through the room

Follow the current Advent of Cyber task in order. Inspect the supplied request, identify the vulnerable input, use the specific technique requested by the room, and verify the output before submitting an answer. If the lab has changed since a guide was written, use the current response and task text as the source of truth.

## What I took from this room

SQL injection is about crossing the boundary between data and executable query syntax. Understanding the query context and validating the result leads to a stronger explanation than copying a payload. In the public version, any confirmed flag should be redacted as flag{hidden}.