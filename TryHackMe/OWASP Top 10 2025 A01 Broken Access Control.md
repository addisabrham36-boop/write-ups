# OWASP Top 10 2025: Broken Access Control

Room: https://tryhackme.com/room/owasptopten2025one

Broken access control occurs when an application does not properly enforce what a user is allowed to do. A user might access another person's record, perform an administrator-only action, or bypass a restriction by changing a request.

## How I think about it

Authentication asks who the user is. Authorisation asks what that user is allowed to access. A user being logged in does not mean they should be able to access every object or function.

In an authorised lab, I compare what a normal user should be able to access with what happens when I change an object identifier or request a restricted route. I should use only the test accounts and data provided by the room.

## Prevention

The server should check permissions for every sensitive action and object. Hiding a button in the browser is not access control, because a user can send a request directly. Applications should default to denying access unless a valid permission check allows it, and tests should cover horizontal and vertical privilege boundaries.

## What I took from this room

Access control must be enforced on the server for every relevant request. A secure interface can still have a serious vulnerability if the backend trusts a user-supplied identifier or assumes that a hidden button prevents access.