# OWASP Top 10 2025: IAAA Failures — TryHackMe Walkthrough

Official room: https://tryhackme.com/room/owasptopten2025one

## Room Overview & Information

**Category:** Web application security  
**Difficulty:** Easy  
**Topics:** A01 Broken Access Control, A07 Authentication Failures, and A09 Logging and Alerting Failures.

IAAA stands for Identity, Authentication, Authorisation, and Accountability. These controls form a sequence: identify the account, prove control of that identity, decide what the identity may do, and record what happened. If one stage is weak, an attacker may access another user's data, take over an account, or act without leaving useful evidence.

## Task 1: Introduction

The room covers three OWASP Top 10 2025 categories:
- **A01 Broken Access Control:** the application does not enforce who can access which object or action.
- **A07 Authentication Failures:** the application does not reliably verify or bind a user's identity.
- **A09 Logging and Alerting Failures:** the application does not record or alert on security-relevant activity.

## Task 2: What is IAAA?

### Question: “What does IAAA stand for?”

**Answer:** Identity, Authentication, Authorisation, Accountability.

- **Identity:** the unique account representing a person or service.
- **Authentication:** proving control of that identity, for example with a password, passkey, or one-time code.
- **Authorisation:** deciding which resources and actions the authenticated identity may use.
- **Accountability:** recording and alerting on who did what, when, and from where.

## Task 3: A01 Broken Access Control

Broken access control occurs when the server does not properly enforce who can read or modify a resource on every request. A common example is an Insecure Direct Object Reference (IDOR), where changing an object ID exposes another user's data.

Start the static banking site attached to the task. Inspect the URL while viewing your account and note the accountID parameter. Change only that identifier within the lab and compare the returned account details.

### Question: “If you don't get access to more roles but can view the data of another users, what type of privilege escalation is this?”

**Answer:** Horizontal privilege escalation.

The user remains at the same general privilege level but accesses another user's resources. Vertical privilege escalation would involve gaining a higher role or performing an administrator-only action.

### Question: “What is the note you found when viewing the user's account who had more than $ 1 million?”

1. Change the accountID value in the provided static site.
2. Identify the account whose balance exceeds one million dollars.
3. Open that account and read the note field.
4. Submit the exact note shown by the active lab.

The note contains a flag. It is redacted here as `THM{hidden}`.

The fix is to enforce ownership and role checks server-side for every read and write operation. Random or encoded IDs do not replace authorisation.

## Task 4: A07 Authentication Failures

Authentication failures can result from username enumeration, weak passwords, missing rate limits, insecure session handling, or registration logic that treats distinct spellings as the same account. The room's static site demonstrates a case-normalisation flaw.

The task asks you to create a new account whose username differs from admin only by letter case. Use the exact test values supplied by the room:
- Register username: `ADmiN`
- Password: `Password`

After registering, use the site's login form with username `admin` and password `Password`. The lab demonstrates that inconsistent username normalisation can bind the new account to the administrator identity.

### Question: “What is the flag on the admin user's dashboard?”

1. Register the case-variant account as instructed.
2. Log in using the username and password specified by the task.
3. Open the account dashboard.
4. Read the flag in the Account Description field.
5. Submit the value from the live room.

The flag is redacted here as `THM{hidden}`.

Secure applications should canonicalise usernames consistently, enforce unique constraints on the canonical form, and bind sessions to the correct account. Login, registration, password reset, and recovery must use the same identity rules.

## Task 5: A09 Logging and Alerting Failures

Logging and alerting failures occur when security-relevant events are missing, incomplete, poorly retained, or not connected to useful alerts. Without reliable records, defenders may be unable to reconstruct an attack or determine whether a login succeeded.

Open the static log-analysis site attached to the task. Review the suspicious activity records and correlate repeated source addresses, login results, and accessed endpoints.

### Question: “It looks like an attacker tried to perform a brute-force attack, what is the IP of the attacker?”

**Answer:** `203.0.113.45`.

The room shows repeated suspicious activity from this address. In a real incident, an IP address is an indicator, not proof of a person's identity. Shared hosting, VPNs, proxies, and compromised devices can complicate attribution.

### Question: “Looks like they were able to gain access to an account! What is the username associated with that account?”

**Answer:** `admin`.

Identify the successful login event in the provided records. The room indicates that a POST request returned status code 200, which is the event to investigate. A 200 response alone is not always proof of authentication success, so read the event details and correlate the username.

### Question: “What action did the attacker try to do with the account? List the endpoint the accessed”

**Answer:** `/supersecretadminstuff`.

This endpoint is the path recorded in the successful account activity. The lesson is to retain the request path and authentication outcome so investigators can reconstruct what happened.

## Task 6: Conclusion and remediation

### Broken access control
- Check ownership and role permissions on every request.
- Do not trust client-supplied object IDs.
- Test cross-user access with separate accounts.

### Authentication failures
- Normalise usernames consistently.
- Enforce uniqueness on the canonical identity.
- Rate-limit failed logins and reset attempts.
- Use MFA and secure session handling.
- Rotate sessions after password or privilege changes.

### Logging and alerting failures
- Log successful and failed authentication events.
- Record password, MFA, and role changes.
- Centralise logs off-host with controlled access and retention.
- Alert on brute-force bursts and unusual privilege changes.
- Include timestamps, account, source, outcome, and endpoint.

## Key takeaways

The room shows three failures of IAAA: an IDOR allows horizontal access to another user's data, inconsistent identity handling allows account takeover, and incomplete logging makes an attack harder to investigate. Verify each result in the active static site and redact flags in public notes.