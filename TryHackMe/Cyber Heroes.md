# CyberHeroes: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/cyberheroes

## Room Overview & Information

**Category:** Web application security  
**Difficulty:** Easy  
**Objective:** Find a way to log in to the CyberHeroes page and uncover the flag.

This is a small client-side authentication challenge. The key weakness is that the login decision is implemented in JavaScript delivered to the browser. Client-side code is visible and can be inspected or modified by the user, so it must never be the authority that protects a secret page.

## Phase 1: Start the lab and open the website

Start the machine and copy the current IP address shown by TryHackMe. Do not reuse the address from another walkthrough.

Open the site in the AttackBox browser:

\`\`\`text
http://MACHINE_IP/
\`\`\`

If the page does not load, confirm the machine has finished booting and that the AttackBox or VPN is connected. Open the login page from the site's own link or navigate to:

\`\`\`text
http://MACHINE_IP/login.html
\`\`\`

The page asks for login details. Before guessing passwords, inspect how the page performs authentication.

## Phase 2: Inspect the source code

Open the login page source using the browser's **View Page Source** option or press Ctrl+U. Search the source for the login form and JavaScript function named `authenticate`.

The room's script checks two variables:
- `a` represents the username input.
- `b` represents the password input.

The comparison includes the username `h3ck3rBoi` and a call to a string-reversal function for the password. This is the important clue: the password is not being verified by a server-side authentication endpoint. The browser receives the logic needed to decide whether the login is accepted.

## Phase 3: Reverse the password string

The source contains the string:

\`\`\`text
54321@terceSrepuS
\`\`\`

The function reverses the string before comparing it with the supplied password. Reverse the characters from end to beginning:

\`\`\`text
SuperSecret@12345
\`\`\`

This gives the password expected by the client-side function. The username shown in the same comparison is:

\`\`\`text
h3ck3rBoi
\`\`\`

These are credentials for the intentionally vulnerable CyberHeroes lab only. Do not reuse them on other services.

## Phase 4: Log in and retrieve the flag

Return to the login form and enter the values found in the source:
- Username: `h3ck3rBoi`
- Password: `SuperSecret@12345`

Submit the form. If the page accepts the credentials, follow the resulting page and read the flag displayed by the active room. Submit the flag in TryHackMe. This public write-up deliberately uses `flag{hidden}` instead of publishing the live flag value.

## Why this works

The application treats JavaScript running in the user's browser as a security boundary. That is a design flaw. Users can inspect page source, alter JavaScript, change form values, or bypass the client-side function entirely. Obfuscating or reversing a string does not protect a password when the comparison and the data are shipped to the client.

A secure implementation would submit credentials to a server over HTTPS. The server would verify a salted password hash, create a session after successful authentication, and enforce authorisation on every protected request. The protected content should not be delivered merely because client-side code sets a success variable.

## Phase 5: Remediation

- Move authentication decisions to the server.
- Store password hashes using a suitable password-hashing algorithm such as Argon2id, scrypt, or bcrypt.
- Use secure session cookies and invalidate sessions on logout.
- Enforce authorisation on every protected route and API request.
- Do not embed passwords, API keys, or secret flags in HTML or JavaScript.
- Use HTTPS and appropriate rate limiting.
- Add tests proving that directly requesting the protected page without a valid session is rejected.

## Key takeaways

CyberHeroes is solved by inspecting the login page source rather than brute-forcing the form. The JavaScript exposes both the expected username and the reversible password check. The deeper lesson is that client-side checks are for user experience, not security; the server must make and enforce the authentication decision.

## Expanded web challenge investigation

### 1. Build a request map

Open the challenge application and inspect its pages, forms, API calls, and JavaScript. Use the browser Network panel to identify the exact request that produces the behaviour described by the room. Record the route, method, parameters, and response shape before testing.

### 2. Identify the trust boundary

Determine which values come from the browser and which decisions must be made by the server. Never assume a value is trusted because it is stored in a hidden input, URL parameter, local storage, or client-side object. The server must validate identity, permissions, and business rules independently.

### 3. Test one hypothesis at a time

Capture a baseline request in an intercepting proxy. Modify a single field and compare the response. If the issue concerns authorization, use only the test users provided by the room. If it concerns input processing, use the task's harmless test value and verify an actual effect rather than relying only on status codes.

### 4. Validate and explain the result

A good write-up ties each conclusion to the request that caused it. Include a sanitized request/response excerpt, the condition that was bypassed, and the impact. Do not include session tokens or unrelated personal data.

### 5. Fix and retest

Apply server-side validation, least-privilege access, safe query APIs, explicit authorization checks, and appropriate logging. Retest both the previously successful attack path and the legitimate user flow to ensure that the fix prevents abuse without breaking intended behaviour.

### 6. Room answers

Use the active challenge to obtain exact endpoint names, values, and flags. Do not copy a guessed answer from a similarly named room. Redact public flags as `flag{hidden}`.
