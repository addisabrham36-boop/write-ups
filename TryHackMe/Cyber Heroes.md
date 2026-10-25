# CyberHeroes: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/cyberheroes

## Room Overview & Information

**Category:** Web application security  
**Difficulty:** Easy  
**Objective:** Inspect client-side authentication code, recover the lab's expected credentials, and retrieve the flag.

CyberHeroes is a client-side authentication challenge. The important weakness is that the login decision is implemented in JavaScript delivered to the browser. Client-side code can be inspected and modified, so it cannot be trusted to protect a secret page.

## Phase 1: Reconnaissance & Enumeration

### 1. Start the machine and inspect the web application

Start the room machine and copy the current IP address. Do not reuse an address from an older session. Open the site in the AttackBox browser:

```text
http://MACHINE_IP/
```

If required, open the login page:

```text
http://MACHINE_IP/login.html
```

If the site fails to load, confirm that the machine has finished booting and that the AttackBox or VPN is connected. Use the website's own links to discover paths rather than guessing unrelated endpoints.

### 2. Inspect the page source

Use the browser's View Page Source command or Ctrl+U. Search for the login form and the JavaScript function named authenticate. The code checks two variables:
- **a** represents the username input.
- **b** represents the password input.

The comparison includes the username h3ck3rBoi and calls a string-reversal function for the password. The crucial point is that the browser is given the logic required to decide whether authentication succeeds.

## Phase 2: Vulnerability Analysis & Initial Access

### 3. Reverse the password string

The JavaScript source contains this value:

```text
54321@terceSrepuS
```

Reverse its characters from end to beginning:

```text
SuperSecret@12345
```

This is the password expected by the client-side check. The corresponding username in the same comparison is:

```text
h3ck3rBoi
```

These credentials apply only to the deliberately vulnerable room. Never reuse a lab password on another service.

### 4. Confirm the authentication weakness

Return to the login form and enter the values found in the source:
- Username: **h3ck3rBoi**
- Password: **SuperSecret@12345**

Submit the form and follow the resulting page. If it accepts the values, read the flag displayed by the active room. This public write-up uses **flag{hidden}** rather than publishing the instance's flag.

The issue is not that reversing a string is a strong password recovery technique. It is that the expected password and comparison logic are shipped to the client. An attacker can inspect the script, modify the function, or bypass it entirely.

## Phase 3: Post-Exploitation and Security Boundary Validation

This room does not provide a host shell or a separate privilege-escalation path. The relevant post-authentication check is whether the protected content is actually enforced by the server. Try opening the protected route directly in a fresh browser session as the room permits. If the content can be retrieved without server-side authentication, that confirms the client-side check is not an effective access-control boundary.

A secure design submits credentials to a server over HTTPS. The server validates a stored password hash, creates a protected session, and enforces authorization on every subsequent request. The protected data should not be delivered simply because a client-side JavaScript variable indicates success.

## Phase 4: Room Questions & Flags Walkthrough

| Objective | Answer or evidence |
|---|---|
| Which page should be inspected? | The login page and its JavaScript source |
| What is the username checked by the script? | **h3ck3rBoi** |
| What string appears before reversing the password? | **54321@terceSrepuS** |
| What password results after reversing it? | **SuperSecret@12345** |
| How is the flag obtained? | Submit the lab credentials, follow the accepted page, and read the flag; redacted here as **flag{hidden}** |

Verify the content from the live machine. The flag value is not copied into this note.

## Phase 5: Key Takeaways & Remediation

- Perform authentication and authorization on the server.
- Store passwords using a suitable password-hashing scheme such as Argon2id, scrypt, or bcrypt.
- Use secure session cookies and invalidate sessions on logout.
- Enforce authorization on every protected route and API request.
- Never embed passwords, API keys, or secret flags in HTML or JavaScript.
- Use HTTPS, rate limits, and monitoring for authentication endpoints.
- Add tests proving that directly requesting a protected page without a valid session is rejected.

CyberHeroes is solved by inspecting the source instead of brute-forcing the login form. The deeper lesson is that client-side checks are for user experience, not security; the server must make and enforce the authentication decision.