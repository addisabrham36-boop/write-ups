# OWASP Top 10 2025: A01 Broken Access Control

Official room: https://tryhackme.com/room/owasptopten2025one

## Room Overview & Information

**Category:** Web application security  
**Difficulty:** Beginner  
**OWASP category:** A01 Broken Access Control  
**Scenario:** Inspect a simulated banking site, identify another user's account through a client-controlled account identifier, and understand why the server must enforce object-level permissions.

This is a focused task within TryHackMe's IAAA Failures room. The static website is a deliberately vulnerable lab. Only manipulate the identifiers in that lab, not a real banking website.

## Phase 1: Understand the IAAA model

- **Identity:** the account representing a person or service.
- **Authentication:** proving control of that identity.
- **Authorisation:** deciding what the identity may read or change.
- **Accountability:** recording who did what, when, and from where.

These stages depend on one another. A user can be authenticated correctly and still be forbidden from viewing another customer's account. A session cookie proves something about the session; it does not grant unlimited access.

## Phase 2: Inspect the account page

Start the static site attached to the room. Inspect the URL while viewing your account. The task uses an `accountID` value in the URL. Record the initial value and the information shown for your current account.

For a real assessment, I would use browser Developer Tools or Burp Suite to inspect the request. The relevant question is whether the server checks the current session's ownership of the requested account, rather than blindly trusting the client-supplied identifier.

## Phase 3: Test object-level authorisation

Within the lab, change the `accountID` value to another value and inspect the returned account. Change only that identifier so the comparison is controlled. If the application returns another user's data without an authorisation check, it demonstrates an Insecure Direct Object Reference (IDOR), a common form of broken access control.

Continue the task until you identify the account with more than one million dollars. Open that account and read the note attached to it. Do not guess the ID or note from another guide; use the current lab's displayed values.

### Question 1

**“If you don't get access to more roles but can view the data of another users, what type of privilege escalation is this?”**

Answer: **Horizontal privilege escalation.**

The user remains at the same general privilege level but accesses another user's resources. Vertical escalation would involve gaining a higher role or performing an administrator-only action.

### Question 2

**“What is the note you found when viewing the user's account who had more than $ 1 million?”**

Procedure:
1. Continue testing the `accountID` value in the supplied static site.
2. Identify the account whose balance exceeds $1 million.
3. Open that account's page.
4. Read the note field.
5. Submit the exact note text shown by the lab.

The note is instance-specific lab content, so this public write-up intentionally explains how to obtain it rather than inventing a value.

## Phase 4: Validate the finding

A valid finding needs more than a changed URL. Verify that the account data belongs to a different user and that the server returns it despite the current user's lack of permission. Keep a record of the original and modified request, the relevant response, and the impact.

Do not attempt to alter balances or access more data than the room requires. Read-only proof is sufficient for this task.

## Phase 5: Remediation

- Derive the acting user from the authenticated server-side session.
- Check ownership or role permissions on every request.
- Do not trust a client-supplied account identifier as proof of authorisation.
- Apply the same checks to read, update, delete, export, and API operations.
- Add automated tests for cross-user and cross-role access.
- Log repeated denied access attempts and alert on suspicious patterns.

Random IDs and encoded IDs may reduce guessing but do not replace authorisation checks.

## Key takeaways

Broken access control is a server-side decision failure. The practical demonstrates horizontal privilege escalation: a normal user can see another user's data by manipulating an object reference. The correct fix is to check the current user's permission for every requested object and action.

## Expanded authorization testing workflow

### 1. Build an access-control matrix

List the user roles, objects, and actions in the application. For each role, state which actions should be allowed and denied. Include read, create, update, delete, export, and administrative operations where they exist. The expected policy should come from the room scenario, not from assumptions.

### 2. Establish a baseline

Log in with the supplied lab account and capture one legitimate request. Record the endpoint, HTTP method, object identifier, session context, and response. Then determine whether changing the object identifier or invoking an alternate endpoint changes which record is returned.

Only test the room's accounts and data. Never manipulate identifiers on a real service without authorization.

### 3. Test horizontal and vertical authorization

Horizontal access-control failures occur when one user can access another user's resource at the same privilege level. Vertical failures occur when a lower-privileged user can perform an action reserved for a higher-privileged role. Missing function-level checks may allow a normal user to call an admin endpoint directly.

A client-side hidden button is not an authorization control. Repeat the request directly and verify whether the server checks the current identity and permission for that exact object and action.

### 4. Validate the result

Compare the baseline and modified request. A changed identifier that returns a successful status is not automatically proof of unauthorized access. Inspect whether protected data was disclosed or a restricted action actually occurred. Record the minimum necessary evidence.

### 5. Fix the root cause

Perform authorization checks server-side on every request. Use deny-by-default policy, derive ownership from the authenticated session rather than trusting client-supplied user IDs, enforce role and object permissions, and test direct API calls. Avoid relying on unpredictable identifiers as the only control; random IDs do not replace authorization.

### 6. Complete the room questions

Map each question to the affected route, object, role, and missing permission check. Use the live lab to retrieve exact values and outcomes. Do not copy another instance's response; redact flags in public notes as `flag{hidden}`.
