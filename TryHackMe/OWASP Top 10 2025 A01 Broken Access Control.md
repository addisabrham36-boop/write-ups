# OWASP Top 10 2025: A01 Broken Access Control

Room: https://tryhackme.com/room/owasptopten2025one

Broken Access Control occurs when an application does not enforce what an authenticated user is allowed to view or do. The issue is usually in server-side authorisation: the browser may hide a button, but the server must still reject a request the user is not permitted to make.

## 1. Understand identity, authentication, authorisation, and accountability

These concepts are related but different:

- **Identity:** the account or service that represents an actor.
- **Authentication:** evidence that the actor controls that identity.
- **Authorisation:** the actions and data that identity is allowed to access.
- **Accountability:** records that show who did what and when.

A user can authenticate successfully and still be unauthorised to access another user's records. A valid session does not grant access to every object.

## 2. Test object-level access in the room

The room's practical uses an account identifier in the URL. Start the static site supplied by the task and inspect the account page. Note the accountID value and the data displayed for the logged-in user.

In the authorised exercise, change the identifier to another value and observe whether the application returns a different user's account. The question asks you to identify the account with more than one million dollars and read the note attached to that account. Use the actual values shown by the live lab; do not copy an account ID or note from another version.

The vulnerability is an Insecure Direct Object Reference (IDOR) when the application accepts a user-controlled object reference without checking that the current user is allowed to access that object. The identifier does not have to be numeric; changing a UUID or encoded value can expose the same design flaw if authorisation is missing.

## 3. Distinguish horizontal and vertical escalation

- **Horizontal privilege escalation:** a user accesses another user's data or actions at roughly the same privilege level.
- **Vertical privilege escalation:** a user performs actions reserved for a more privileged role, such as an administrator.

Changing an account ID and viewing another customer's data is generally horizontal access-control failure. Reaching an admin-only function as a normal user is vertical escalation.

## 4. Test each sensitive action

For every relevant endpoint, test the authorised user's own resource, another user's resource, and an action that requires a higher role. Keep the request the same except for the identity or object reference being tested. Verify both read and write operations where the lab allows it.

Do not assume that hiding an identifier, using a random UUID, or encoding an ID is a substitute for an access check. The server should validate the current identity's permission for every object and action.

## 5. Remediation

- Enforce authorisation on the server for every request.
- Derive the current user from the authenticated session rather than trusting a client-supplied user ID.
- Check ownership or role before returning or modifying an object.
- Use centralised authorisation logic and test it consistently.
- Log denied access attempts and alert on repeated cross-account access.
- Add automated tests for horizontal and vertical access control.

## 6. Working through the room

Start the static practical, inspect the accountID parameter, and compare the accounts as the task instructs. Submit the account note requested by the room using the live site's value. This write-up explains the method but intentionally does not invent or publish the flag.

## What I took from this room

Access control is enforced by the server, not by the interface. The safest test changes one object reference at a time and verifies whether the server checks the current user's permissions before returning data.