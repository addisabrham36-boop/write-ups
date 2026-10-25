# OWASP Top 10 2025: Injection

Room: https://tryhackme.com/room/owasptopten2025

Injection flaws happen when untrusted input is interpreted as part of a command or query instead of being treated as data. SQL injection is a familiar example, but similar problems can affect operating-system commands, templates, and other interpreters.

## Why it happens

The root issue is usually mixing code and data. If an application builds a query by concatenating raw user input, special characters may change the meaning of the query. The same design mistake can appear when an application builds shell commands from input.

## Safer design

For SQL, parameterised queries separate values from query structure. For operating-system operations, safer APIs are preferable to constructing shell commands, and input should be validated against the expected format. Escaping alone is easy to get wrong when used as the only defence.

## Testing in a lab

I should use the exact input points and test data provided by the room, compare the application's behaviour, and avoid destructive payloads. A strange error can be a clue, but it does not automatically prove that injection is possible.

## What I took from this room

Injection is a trust-boundary problem. Applications should treat user input as data, use safe APIs, and validate the result with testing. The right fix addresses the underlying construction of the query or command rather than blocking one specific string.