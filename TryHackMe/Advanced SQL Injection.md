# Advanced SQL Injection

Room: https://tryhackme.com/room/advancedsqlinjection

This room builds on basic SQL injection and covers second-order injection, filter evasion, out-of-band techniques, injection through HTTP headers, automation, and mitigation. The target is a vulnerable lab application backed by MySQL/MariaDB. Keep all requests inside the room and use the active task's output rather than copying machine-specific values.

## 1. Introduction and quick recap

Start the lab and inspect the provided application and services. The room identifies MySQL on TCP port 3306. A service scan can confirm the environment:

    nmap -sC -sV -p 3306 TARGET_IP

Replace TARGET_IP with the address supplied by the room. A service banner helps identify the database, but the actual application may reach it through a local interface rather than exposing it publicly.

In-band SQL injection uses the same communication channel for the injection and the returned data. Out-of-band SQL injection uses a separate channel, often an HTTP or DNS request, to signal or retrieve information when the normal response does not reveal it directly.

## 2. Second-order SQL injection

A second-order injection occurs when an input is stored safely or appears harmless at first, but is later inserted into another query without proper parameterisation. The dangerous query may execute in a different page or workflow from the one where the input was submitted.

The practical uses a book-review application. The task asks you to update the title of all books to the word “compromised” and observe the resulting flag. It then asks you to drop the table named hello and observe another flag.

Work through the application as the room instructs:
1. Identify where the application stores the value.
2. Determine which later operation uses that value in a query.
3. Inspect the relevant request and response.
4. Follow the practical's instructions to demonstrate the intended database modification.
5. Verify the effect in the application or the task output.

The two requested flags are intentionally not included here. Submit the values shown by the live lab and redact them as flag{hidden} in the public write-up.

The defensive lesson is that data must remain data in every query, including queries executed later. Parameterise both the original write and the later read or update operation.

## 3. Filter evasion

Applications sometimes try to block SQL injection by removing keywords, spaces, or selected characters. Such filters are fragile because SQL syntax supports alternate whitespace, comments, casing, and other equivalent forms. A blacklist can also accidentally transform input into a different dangerous string.

The room asks you to submit invalid characters and identify the database error code. Inspect the exact response returned by the lab. Do not guess the code from a generic MySQL error because it depends on the malformed query and database version.

When a filter blocks one form, identify what the filter actually does before testing another representation. Change one feature at a time and compare the request and response. A filter bypass is meaningful only if the resulting input changes query behaviour.

## 4. Out-of-band SQL injection

Out-of-band techniques are useful when the application does not return query results through its normal HTTP response. If the database can make a network request to an external service, the request may signal that an injected expression executed.

In the room, follow the provided lab instructions and use only the controlled callback mechanism intended by the exercise. The important concepts are:
- The database must support the relevant function.
- Network egress must permit the callback.
- The injected expression must execute in the database context.
- The callback evidence must be correlated with the exact request.

A missing callback does not automatically prove that the input is safe. The function may be unsupported, network egress may be blocked, or the query may not have executed.

## 5. HTTP header injection points

SQL injection is not limited to fields visible in a form. Applications may use request headers such as User-Agent, Referer, or other metadata in database queries for logging, analytics, or application logic. If the server concatenates such values into SQL, a header can become an injection point.

Use the room's request and proxy tooling to identify which header the task asks you to test. Keep other request fields unchanged, observe the response, and use the task's feedback to confirm whether the header reaches a vulnerable query. Do not assume that every header is stored in a database.

## 6. Automation tools

Automation can help test many inputs, but it should follow manual understanding rather than replace it. First identify the parameter or header, the response behaviour, and the lab's permitted scope. Then use the tool and options that match the discovered input.

SQLmap supports multiple techniques and request formats. Its behaviour depends on the endpoint, parameter, cookies, headers, and database response. Use it only against the assigned target and avoid broad options that are unnecessary for the task. If a request requires authentication or custom headers, preserve those details from the authorised lab request.

Automation output is not proof by itself. Confirm the reported finding by understanding the request and the evidence it produced.

## 7. Error interpretation and troubleshooting

- A generic HTTP error may be caused by application validation rather than SQL execution.
- A database error can reveal query structure, but the exact message depends on the engine and query.
- A second-order payload may execute only after a later operation uses the stored value.
- An out-of-band callback requires both successful query execution and a reachable callback channel.
- A header-based test must use the correct request header and endpoint.
- A filter may alter the input before it reaches the database.

Keep a record of the request, the response, the suspected query context, and the conclusion. Do not claim a vulnerability if the evidence is only a single unusual response.

## 8. Prevention

Use prepared statements or parameterised queries for all database operations. Do not concatenate untrusted input into SQL, including values stored for later use or copied from HTTP headers. Use least-privilege database accounts, restrict database network access, and avoid exposing detailed SQL errors to users.

Input validation can enforce expected formats, but it is not a substitute for parameterisation. Log security-relevant failures without storing passwords or sensitive query data unnecessarily. Add regression tests for normal inputs, malformed inputs, stored values, and alternate request paths.

## What I took from this room

Advanced SQL injection demonstrates that vulnerable input may be used later, hidden behind a filter, or supplied through a header instead of a form. The right response is to understand the query context, test the specific lab behaviour, verify the result, and fix the root cause with parameterised queries and least privilege.

## Phase 4: Exact room-question walkthrough

The values below are documented for the standard room version. A live room is authoritative if its data changes. Use the practicals to understand how each value is found instead of treating the table as a substitute for testing.

### Task 1: Introduction

**Question: “What is the port on which MySQL service is running?”**

**Answer:** `3306`.

MySQL and MariaDB commonly use TCP 3306. In the lab, the service can be identified by the room's information or service scan. A database port can be internal to the web application and need not be exposed to the public interface.

### Task 2: Quick Recap

**Question: “What type of SQL injection uses the same communication channel for both the injection and data retrieval?”**

**Answer:** `In-band`.

The injected request and returned database result travel through the same channel, such as the HTTP request and response.

**Question: “In out-of-band SQL injection, which protocol is usually used to send query results to the attacker's server?”**

**Answer:** `HTTP`.

Out-of-band injection uses a separate channel, commonly HTTP or DNS. The exact technique depends on database features and network egress.

### Task 3: Second-Order SQL Injection

A second-order issue occurs when input is stored and becomes dangerous only when a later query reuses it. The initial submission may appear harmless because it is not immediately interpolated into executable SQL.

**Question: “What is the flag value after updating the title of all books to ‘compromised’?”**

**Flag:** Read the value displayed by the active lab after the title-update operation. Public copy: `THM{hidden}`.

Follow the book-review practical and use its intended query path to perform the mass title update. Verify the UI or task output shows that the operation occurred before submitting the flag.

**Question: “What is the flag value once you drop the table hello from the database?”**

**Flag:** Read the value displayed by the active lab after the table-drop task. Public copy: `THM{hidden}`.

This is a destructive database operation intended only for the disposable room. It demonstrates the impact of injection on database integrity. In production, least-privilege database accounts should not have schema-modification permissions unless the application requires them.

### Task 4: Filter Evasion Techniques

**Question: “What is the MySQL error code once an invalid query is entered with bad characters?”**

**Answer:** `1064`.

MySQL/MariaDB uses error 1064 for a SQL syntax error. To find it, submit the malformed input that the room requests and inspect the returned database error. The error code depends on the syntax and database implementation; do not assume all invalid input produces 1064.

**Question: “What is the name of the book where book ID=6?”**

**Answer:** `Animal Series`.

Inspect the result for book ID 6 in the practical. Filter evasion is a demonstration that attempting to remove a few characters or keywords is not equivalent to using parameterized statements.

### Task 5: Filter Evasion Techniques, Continued

**Question: “What is the password for the username ‘attacker’?”**

**Answer:** `tesla`.

Use the filter-evasion exercise to query the intended user record. Keep the result tied to the room database; this is a weak lab credential and must not be tested on unrelated systems.

**Question: “Which option can be used if the SELECT keyword is banned?”**

**Answer:** `c`, meaning both `SElect` and `SeLect`.

SQL keywords are generally case-insensitive in MySQL/MariaDB syntax, so a case-only denylist can be bypassed. This is not a durable defense. Parameterization removes the underlying syntax/data confusion.

### Task 6: Out-of-band SQL Injection

**Question: “What is the output of @@version on the MySQL server?”**

**Answer:** `10.4.24-MariaDB`.

Read the version value from the room's query output or the out-of-band evidence path. The value identifies the database build used by the standard lab deployment.

**Question: “What is the value of @@basedir?”**

**Answer:** `C:/xampp/mysql`.

This is the base directory value reported by the room's database. Treat it as a lab-specific result rather than a universal MariaDB path.

### Task 7: Other Techniques

**Question: “What is the value of the flag field in the books table where book_id = 1?”**

**Flag:** Read the value shown for the requested database field in the active lab. Public copy: `THM{hidden}`.

Use the task's described database query or injection point to identify the row with book_id 1 and read the flag field. Preserve the exact braces and case.

**Question: “What field is detected on the server side when extracting the user agent?”**

**Answer:** `User-Agent`.

The HTTP User-Agent header is a common source of client metadata. A header becomes an injection point only if the application later uses it unsafely in a SQL query.

### Task 8: Automation

**Question: “Does the dynamic nature of SQL queries assist a pentester in identifying SQL injection (yea/nay)?”**

**Answer:** `nay`.

Dynamic query construction is not itself a security signal. The issue is whether untrusted input is concatenated into executable SQL and whether safe parameter binding separates data from code. Automated scanners may identify candidates, but manual verification still matters.

### Task 9: Best Practices

**Question: “What command does MSSQL support to execute system commands?”**

**Answer:** `xp_cmdshell`.

This SQL Server feature can run operating-system commands when enabled and sufficiently privileged. It is commonly disabled because it increases risk. MySQL/MariaDB has different mechanisms and should not be assumed to support the same procedure.

## Practical technique reference

| Technique | What to observe | What it demonstrates |
|---|---|---|
| Second-order SQLi | Input stored, then used by a later query | Stored input can become dangerous later |
| Filter evasion | Filtered input changes when the parser sees equivalent syntax | Blocklists do not reliably neutralize SQL syntax |
| Out-of-band SQLi | Separate callback from a database-side operation | Results can travel through a channel other than the page response |
| Header-based SQLi | Header value reaches a query unsafely | Inputs other than visible form fields can reach SQL sinks |
| Automation | Scanner report followed by manual confirmation | Tools prioritize tests; evidence confirms the finding |
