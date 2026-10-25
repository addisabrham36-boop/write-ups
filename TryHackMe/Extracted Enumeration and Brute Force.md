# Enumeration & Brute Force: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/enumerationbruteforce

## Room Overview & Information

**Category:** Web application penetration testing  
**Difficulty:** Beginner to intermediate  
**Focus:** Authentication enumeration, verbose errors, predictable password-reset tokens, HTTP Basic Authentication, Burp Suite Intruder, and OSINT.

This room contains multiple small web labs under the `enum.thm` hostname. The important pattern is to understand how the application responds before automating requests. Run the exercises only against the machine assigned by TryHackMe. Do not use these techniques against a public login page without explicit written permission.

## Phase 1: Reconnaissance & Enumeration

### Task 1: Introduction and setup

Start both the attacker machine and lab machine as instructed. Record the generated target IP. Add the host mapping to `/etc/hosts`:

```text
MACHINE_IP enum.thm
```

Replace `MACHINE_IP` with the current lab address. The mapping allows the browser to request the intended virtual host instead of the web server's default site. Wait for the target to finish booting, then open:

```text
http://enum.thm
```

If the page does not load, check the IP and hosts entry, confirm the VPN or AttackBox is connected, and allow the room's startup time. Do not copy an address from an old walkthrough.

## Task 2: Authentication Enumeration

Authentication enumeration identifies information leaked by login, registration, or password-reset workflows. For example, an application might respond with “Email does not exist” for an unknown account and “Invalid password” for an existing account. This difference confirms which addresses are registered.

### Question: “What type of error messages can unintentionally provide attackers with confirmation of valid usernames?”

The intended concept is **verbose or user-enumerating error messages**. These errors distinguish an unknown username from a wrong password and reveal account existence.

### Inspect the verbose-login practical

Open the lab's verbose login page:

```text
http://enum.thm/labs/verbose_login/
```

Submit an email address that is not registered and note the response. Then submit a known registered address with an incorrect password, as instructed by the room. Compare the response text.

The lab's example distinguishes an unknown email with a message like “Email does not exist” from an existing email with “Invalid password.” The difference is the leak. In a production application, both cases should normally return a generic response that does not disclose whether the account exists.

### Automate the check with the room's script

The room provides a Python script that sends POST requests to the lab endpoint and classifies responses. Save the supplied script as `script.py`, install the `requests` library in the AttackBox if needed, and use the username list provided by the room:

```bash
python3 -m pip install requests
python3 script.py usernames_gmail.com.txt
```

Use a virtual environment if the system Python is externally managed. The list filename above is the one shown in the room example; use the actual file you downloaded. The script reads each candidate, sends it to the lab's login function, and looks for the known invalid-account error. A response that differs is marked as a candidate valid address.

The result is only as reliable as the error-string comparison. If the application changes its message or returns an unexpected error, the script may misclassify an account. Verify a candidate manually in the lab.

### Question: “What is the valid email address from the list?”

Run the supplied script against the room's list. Find the entry marked `[VALID]` and submit that exact email address. The actual address is deliberately not hard-coded here because the room's provided list and active instance are the source of truth.

### Task 3: Enumerating Users via Verbose Errors

Verbose errors can disclose internal paths, database details, usernames, or application logic. For this lab, the login response tells the tester whether an email exists.

A repeatable test looks like this:

1. Send a request with an invalid email and record the response.
2. Send the same request with a candidate email and the same invalid password.
3. Compare status, body, response length, and timing.
4. Repeat only when needed to rule out transient network effects.
5. Confirm the difference is stable before drawing a conclusion.

The room also shows a Python script. The important components are:
- `requests` sends HTTP requests.
- The endpoint points to the lab's login handler.
- The `headers` dictionary mirrors the expected browser request.
- The `data` dictionary submits the candidate email and a placeholder password.
- The response JSON is checked for the known “Email does not exist” message.
- The main loop reads candidate addresses from a text file.

If the script fails, check the hostname mapping, the endpoint path, the request field names, and whether the response is JSON. Do not assume the script's result is correct if the server returns an error or the lab is still starting.

### Task 4: Exploiting Vulnerable Password Reset Logic

Password-reset systems often send a single-use token or code to the user's registered channel. A secure token should be unpredictable, time-limited, bound to the correct account, and invalidated after use. A short numeric token with a small range is vulnerable to guessing.

The room's example code uses a random integer between 100 and 200. That range contains only 101 possible values, so it is unsuitable for a security token.

### Prepare the reset request

Open the predictable-token lab:

```text
http://enum.thm/labs/predictable_tokens/
```

Enter the target email specified by the room, `admin@admin.com`, into the reset form and submit it. The practical returns a success message and demonstrates a reset URL with a numeric token parameter.

### Generate the candidate tokens

The room uses Crunch to create every three-digit candidate from 100 through 200:

```bash
crunch 3 3 -o otp.txt -t %%% -s 100 -e 200
```

Parameter breakdown:
- `3 3` sets the minimum and maximum output length to three characters.
- `-o otp.txt` writes the generated list to `otp.txt`.
- `-t %%%` uses three digit placeholders.
- `-s 100` sets the starting value.
- `-e 200` sets the ending value.

This creates 101 candidate values. Use the list only against the room's deliberately vulnerable reset endpoint.

### Test the token with Burp Suite Intruder

1. Open Burp Suite and configure the browser to use its proxy.
2. Visit the reset URL provided by the lab.
3. Capture the request and send it to **Intruder**.
4. In the Positions tab, mark only the `token` parameter as the payload position.
5. In the Payloads tab, load `otp.txt`.
6. Start the attack and compare response status, length, and body.
7. Identify the candidate whose response differs from the invalid-token responses.
8. Use the successful reset flow to log in as the lab directs.

A response with a different content length can be a useful clue, but it is not proof by itself. Inspect the response body and confirm that the reset succeeded. Burp Community Edition may process the list more slowly than the Professional version.

### Question: “What is the flag?”

After the successful reset, log in using the new password and open the account dashboard. Copy the flag displayed by the lab and submit it. This public write-up intentionally uses `flag{hidden}` rather than publishing the live flag.

### Task 5: Exploiting HTTP Basic Authentication

HTTP Basic Authentication sends a Base64 representation of `username:password` in the `Authorization` header. Base64 is encoding, not encryption. Basic authentication must be protected by HTTPS to prevent credentials being exposed in transit.

The header format is:

```text
Authorization: Basic BASE64(username:password)
```

Open the lab page:

```text
http://enum.thm/labs/basic_auth/
```

Enter any lab-only credentials in the browser prompt and capture the request in Burp. Send it to Intruder and inspect the Authorization header.

### Configure the payload

1. Decode the Base64 credentials to identify the username and separator.
2. In Intruder, set the payload position to the credential value.
3. Use the wordlist provided by the room, such as `500-worst-passwords.txt`.
4. Configure payload processing to prepend the lab username and a colon to each candidate password.
5. Configure Base64 encoding so each `username:password` pair becomes the required header value.
6. Remove the padding character only if the room's specific Intruder configuration calls for that setting.
7. Run the attack against the lab endpoint and inspect the response codes.

The room demonstrates that a successful request returns HTTP status 200. Do not treat every 200 response as success without checking the page and response body. Decode the successful Authorization value, confirm the credentials, and log in through the intended lab interface.

### Question: “What is the flag?”

After logging in with the valid lab credentials, copy the flag from the dashboard and submit it. Redact the flag in public notes.

### Additional task: “Try using Hydra instead of Burp to brute force the password.”

Hydra can test authentication mechanisms when the service, username, wordlist, and failure condition are known. For HTTP Basic Authentication, the general command shape is:

```bash
hydra -l USERNAME -P /path/to/wordlist.txt enum.thm http-get /labs/basic_auth/
```

- `-l USERNAME` supplies one username.
- `-P` selects a password list.
- `enum.thm` is the lab host.
- `http-get` tells Hydra to test HTTP Basic authentication using GET requests to the specified path.

Use the exact username and failure condition from the active lab. Tool syntax can vary by service and Hydra version; if the command does not work, verify the module options with `hydra -U http-get` or the installed documentation. Keep request rates reasonable and test only the TryHackMe target.

## Phase 3: Post-Exploitation and Additional Reconnaissance

The authentication labs do not provide an operating-system shell or a privilege-escalation path. This phase therefore focuses on validating the results, limiting credential exposure, and keeping the later OSINT activity passive and in scope.

### Task 6: OSINT

The room introduces the Internet Archive's Wayback Machine and Google dorking. Older snapshots may expose paths or files that are no longer linked from the current site. Historical data can be stale, so validate whether the resource still exists before treating it as a current exposure.

### Wayback Machine

Search the authorised domain in the Wayback Machine to inspect archived versions. The room also demonstrates `waybackurls`, a tool that lists URLs captured by the archive.

If you install it from its official repository in your own lab environment, build it according to the current project instructions. A typical workflow is:

```bash
waybackurls example.com
```

Replace `example.com` with the authorised domain. Review the results for old routes, parameters, and file paths. An archived URL is not proof that the current server still serves the resource.

### Google dorks

Examples shown by the room include:

```text
site:example.com inurl:admin
filetype:log "password" site:example.com
intitle:"index of" "backup" site:example.com
```

- `site:` restricts results to a domain.
- `inurl:` searches for a term in the URL.
- `filetype:` narrows results by file type.
- `intitle:` searches page titles.
- Quoted phrases search for exact wording.

Use these operators only for assets in scope. Do not download, disclose, or exploit sensitive information discovered through search results.

### Question: “Click me to proceed to the next task.”

This is a completion prompt rather than a technical answer. Select the continuation option after reviewing the task content.

## Phase 4: Room Questions & Answers

The email and flag answers that depend on the live lab remain instance-bound. Use the exact candidate marked by the supplied script and retrieve flags after completing the reset or login flow. The question text and validation method are preserved in the task sections above.

### Task 7: Conclusion

The room teaches how verbose errors and weak reset logic can reveal valid accounts or allow password recovery, and how Basic Authentication can be attacked when credentials are weak. It also demonstrates using public archives and search operators for OSINT.

### Question: “I can now attack authentication forms!”

This is the room's final acknowledgement. Submit it as the task requests after completing the exercises.

## Phase 5: Key Takeaways & Remediation

- Use generic responses for login and reset flows so account existence is not revealed.
- Rate-limit authentication and reset requests and monitor for repeated failures.
- Generate reset tokens with a cryptographically secure random generator and a sufficiently large keyspace.
- Bind tokens to one account, expire them quickly, and invalidate them after use.
- Require HTTPS for Basic Authentication and prefer stronger, well-managed authentication mechanisms where appropriate.
- Avoid predictable passwords and support MFA.
- Review public archives and search-engine indexes for accidental exposure.
- Keep testing restricted to authorised systems.

## Key takeaways

Enumeration reduces uncertainty for an attacker; predictable reset tokens turn a recovery feature into an authentication bypass; and Basic Authentication is only as strong as the credentials and transport protection around it. The analyst should prove each weakness from the actual response, verify the successful path, and recommend a fix that removes the underlying design flaw.

## Phase 5: Task evidence and answer checklist

| Lab | Evidence required | Verification |
|---|---|---|
| Verbose login errors | Different response for unknown email versus known email | Compare the two response bodies using the same incorrect password |
| Candidate email list | Exact item marked valid by the supplied script | Confirm the script sent requests to the current enum.thm lab |
| Predictable reset token | Successful response for one candidate token | Inspect response body and complete the intended reset flow |
| HTTP Basic Authentication | Authorization header and response for each candidate | Decode the successful header and verify the protected page |
| Hydra alternative | Reported candidate plus application response | Confirm the candidate in the lab rather than relying only on output |
| OSINT | Wayback snapshot or indexed URL from the named domain | Distinguish historical evidence from current exposure |

The task may use generated credentials, addresses, and flags, so retrieve exact values from the active lab. Do not substitute a result from another instance. Where the question depends on the supplied list, the answer is the candidate whose response differs from the documented failure response.

## Defensive remediation checklist

- Return generic authentication and reset responses.
- Use long, random, one-time reset tokens and expire them quickly.
- Rate-limit login and reset endpoints without making lockouts easy to weaponise.
- Require HTTPS and protect session tokens.
- Monitor repeated failed attempts and suspicious successful logins.
- Review publicly indexed files and historical URLs.
- Remove exposed secrets, rotate them, and check version-control history for copies.

The core workflow is: baseline request, change one variable, compare the response, validate the result, document evidence, and recommend a root-cause fix.
