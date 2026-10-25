# Advanced SQL Injection — Advent of Cyber 2024

Room: https://tryhackme.com/room/adventofcyber2024

SQL injection occurs when an application builds a database query in a way that lets untrusted input alter the query's intended structure. An advanced lab may combine several concepts, so I should follow the actual task clues rather than assume a single payload will work everywhere.

## Understand the query context

The behaviour depends on where the input is inserted, the database engine, the query structure, and how errors are handled. A payload that works in one context may fail in another. I should compare responses and change one thing at a time in the authorised lab.

## Defensive lesson

Parameterised queries are the main defence because they keep user-supplied values separate from SQL syntax. Applications should also use least-privilege database accounts, avoid exposing detailed database errors to users, and test sensitive query paths.

## What I took from this room

SQL injection is not a matter of memorising one magic string. It requires understanding the query context and verifying the effect. For the final walkthrough, I should add the exact task steps and observed results from the room, and redact any actual flag as `flag{hidden}`.