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
