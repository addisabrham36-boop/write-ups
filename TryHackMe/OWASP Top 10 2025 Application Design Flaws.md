# OWASP Top 10 2025: Application Design Flaws — TryHackMe Walkthrough

Official room: https://tryhackme.com/room/owasptopten2025two

## Room Overview & Information

**Category:** Web application security  
**Difficulty:** Easy  
**OWASP categories:** A02 Security Misconfigurations, A03 Software Supply Chain Failures, A04 Cryptographic Failures, and A06 Insecure Design.

This room combines short theory sections with four practical web applications. The tasks demonstrate that serious vulnerabilities can arise from deployment mistakes, unverified dependencies, exposed cryptographic keys, and assumptions built into application logic.

Start the practical machine and use the AttackBox or a machine connected to the TryHackMe VPN. Replace MACHINE_IP with the current address shown by the room. Do not reuse an IP from an old session. All flags are redacted in this public write-up.

## Task 1: Introduction

The room covers four categories:

- **A02 Security Misconfigurations:** unsafe defaults, debug output, exposed endpoints, and overly broad permissions.
- **A03 Software Supply Chain Failures:** untrusted or compromised libraries, dependencies, build pipelines, and update artifacts.
- **A04 Cryptographic Failures:** weak algorithms, exposed keys, poor key management, or encryption used incorrectly.
- **A06 Insecure Design:** unsafe assumptions or missing controls in the application's intended workflow.

## Task 2: A02 Security Misconfigurations

### What it means

A security misconfiguration occurs when a service is deployed with unsafe settings, exposed debugging information, unnecessary endpoints, or permissions broader than required. It is a configuration problem rather than necessarily a bug in the main business logic.

### Practical endpoint

Open the user-management API on port 5002:

\`\`\`text
http://MACHINE_IP:5002/
\`\`\`

The page explains the API. Query a normal user record:

\`\`\`bash
curl http://MACHINE_IP:5002/api/user/1
\`\`\`

The normal response contains a user record. The application warns that the user ID must be numeric. Test the error-handling path with an invalid identifier:

\`\`\`bash
curl http://MACHINE_IP:5002/api/user/bad
\`\`\`

The intended vulnerability is that the error response includes debug information and a flag in the error message or traceback. An error should not expose internal variables, stack traces, or secrets to a client.

### Question: “What's the flag?”

1. Open the API on port 5002.
2. Inspect the normal response for user ID 1.
3. Compare it with the response to the invalid ID shown above.
4. Locate the flag in the verbose error/debug output.
5. Submit the value from the live room.

The flag is redacted as `THM{hidden}`.

### Remediation

Return a generic client-facing error, validate input before processing it, and write diagnostic detail only to access-controlled server logs. Disable debug mode in production and ensure errors never include secrets.

## Task 3: A03 Software Supply Chain Failures

### What it means

Software supply-chain failures occur when a dependency, library, model, update, or build process is compromised or not verified. A trusted application can become unsafe because it imports an untrusted component or exposes debugging features from that component.

### Practical endpoint

Open the API on port 5003:

\`\`\`text
http://MACHINE_IP:5003/
\`\`\`

The application uses a helper library and provides a JSON processing endpoint at `/api/process`. The route accepts POST requests and checks whether the submitted data is the string `debug`. In that case, it returns the library's debug information.

Send the request:

\`\`\`bash
curl -X POST \
  -H 'Content-Type: application/json' \
  --data '{"data":"debug"}' \
  http://MACHINE_IP:5003/api/process
\`\`\`

Parameter breakdown:
- **-X POST** selects the HTTP method.
- **-H** sets the JSON content type.
- **--data** supplies the JSON request body.
- The URL identifies the API endpoint in the authorised lab.

The response includes debug information such as a version value and sensitive internal fields, including the task's flag. The problem is not merely that a library is old; the application exposes internal debug data without a production-safe access check.

### Question: “What's the flag?”

1. Open the port 5003 practical.
2. Inspect the page and identify the JSON processing endpoint.
3. Send the POST request with the data value `debug`.
4. Read the returned JSON and locate the flag.
5. Submit the value from your active instance.

The flag is redacted as `THM{hidden}`.

### Remediation

Verify dependencies and their provenance, pin and review versions, protect the build pipeline, and remove debug endpoints from production. Sensitive configuration should never be returned to an unauthenticated client.

## Task 4: A04 Cryptographic Failures

### What it means

Cryptographic failures occur when sensitive data is not adequately protected. A modern algorithm cannot compensate for an exposed key, insecure mode, poor randomness, or incorrect implementation. Keys must be protected separately from the ciphertext.

### Practical endpoint

Open the Secure Document Viewer on port 5004:

\`\`\`text
http://MACHINE_IP:5004/
\`\`\`

The page shows an encrypted document. Inspect the page source and the JavaScript file it references:

\`\`\`text
/static/js/decrypt.js
\`\`\`

The source exposes the encryption configuration:

\`\`\`javascript
const SECRET_KEY = "my-secret-key-16";
const ENCRYPTION_MODE = "ECB";
const KEY_SIZE = 128;
\`\`\`

The encrypted document displayed by the page is:

\`\`\`text
Nzd42HZGgUIUlpILZRv0jeIXp1WtCErwR+j/w/lnKbmug31opX0BWy+pwK92rkhjwdf94mgHfLtF26X6B3pe2fhHXzIGnnvVruH7683KwvzZ6+QKybFWaedAEtknYkhe
\`\`\`

The page's JavaScript also uses `atob()`, which indicates Base64 decoding of the supplied text. Base64 is an encoding, not encryption. The key and mode are exposed in client-side JavaScript, so the ciphertext's confidentiality is lost.

### Decrypt the document

Use the room's suggested CyberChef workflow:
1. Open CyberChef in the AttackBox browser.
2. Add a **From Base64** operation if needed to convert the displayed ciphertext into bytes.
3. Configure AES decryption with the key shown in the source and the mode specified by the application.
4. Use the block mode and key format that match the page's implementation.
5. Inspect the plaintext result and locate the flag.
6. Submit the flag from the active room.

AES-ECB is used by the lab, but ECB is not suitable for many real data-encryption scenarios because equal plaintext blocks produce equal ciphertext blocks. More importantly here, the key is not secret if it is shipped to every browser.

### Question: “What's the flag?”

Decrypt the document using the key and mode found in the supplied JavaScript. The plaintext contains the flag. The public note redacts it as `THM{hidden}`.

### Remediation

Use authenticated encryption such as AES-GCM with secure nonce handling, keep the key on the server or in a proper key-management system, and never ship secret keys to client-side JavaScript. Validate and rotate exposed keys, and re-encrypt data if confidentiality has been compromised.

## Task 5: A06 Insecure Design

### What it means

Insecure design occurs when the system's workflow or trust assumptions are unsafe. Examples include missing authorisation checks, relying on a client application to hide data, or assuming that only a mobile app will access an API.

### Practical endpoint

Open SecureChat on port 5005:

\`\`\`text
http://MACHINE_IP:5005/
\`\`\`

The page describes a chat service, but the task's hint asks whether the developers assumed only mobile devices could access it. Inspect the source and then enumerate the API paths using the lab's intended tools. Existing endpoints from previous tasks may be clues, but the SecureChat API uses its own routes.

A targeted content-discovery command can help find API endpoints:

\`\`\`bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/api/api-endpoints.txt \
  -u http://MACHINE_IP:5005/FUZZ -ac -X POST
\`\`\`

- **-w** selects the API endpoint wordlist.
- **-u** sets the URL, with FUZZ marking the position to test.
- **-ac** enables automatic response calibration.
- **-X POST** tests using POST requests.

Use the wordlist path available in the AttackBox. A status code such as 405 may indicate that a route exists but does not accept the tested method; inspect the response and test the method the application expects.

The room's intended discovery chain includes a user-list endpoint and a messages endpoint for the admin account. Use the exact paths found in the lab and inspect the JSON response. The admin's message contains the flag.

### Question: “What's the flag?”

1. Open the SecureChat practical on port 5005.
2. Inspect the page and enumerate the API routes.
3. Identify the endpoint that lists users and the endpoint that returns the admin's messages.
4. Read the message content that contains the flag.
5. Submit the value from the active room.

The flag is redacted as `THM{hidden}`.

### Remediation

Do not rely on the mobile app or an obscure endpoint as a security boundary. Require authentication and server-side authorisation on every API route, restrict message access to the owning user, and avoid exposing internal records through undocumented endpoints. Threat-model alternate clients and direct API access during design.

## Task 6: Conclusion

The four practicals demonstrate different root causes:
- **A02:** verbose errors expose internal information.
- **A03:** an unsafe debug path reveals sensitive data from a dependency-backed application.
- **A04:** a client-side key and weak encryption design expose the document.
- **A06:** the application assumes that hidden or undocumented APIs will not be called directly.

## Key takeaways

Secure systems need safe defaults, verified dependencies, proper key management, and server-side authorisation. Debug output should not reveal secrets, cryptographic keys must not be shipped to clients, and APIs must enforce permissions regardless of which client calls them.

## Expanded design-review workflow

### 1. Separate design defects from implementation defects

A design flaw exists when the system's intended behaviour or trust model is unsafe, even if the code correctly follows that design. An implementation flaw occurs when code fails to implement a sound design. For example, a workflow that allows password reset without proving account ownership is a design problem; a correctly designed reset flow with a faulty token comparison is an implementation problem.

### 2. Map the business process

Draw the sequence of actors, endpoints, state changes, and trust boundaries. Identify the sensitive action and the proof the system requires before allowing it. Pay particular attention to account recovery, payments, invitations, multi-step approval, and functions that make irreversible changes.

### 3. Test the invariant

An invariant is a rule that must remain true throughout the workflow. Examples include "only the account owner may change the recovery address" and "a payment cannot be approved by the same party that created it." Test whether alternate navigation or repeated requests can violate the invariant. Use only the room's test accounts and objects.

### 4. Verify a business-logic flaw

Capture the normal sequence first. Change one step or state value at a time, then check whether the server revalidates the invariant. A UI error, unexpected redirect, or successful response is not enough; verify the actual state change. Avoid performing real transactions or destructive actions.

### 5. Design controls around the invariant

Enforce permissions and workflow state on the server. Make sensitive operations atomic where possible, use one-time expiring tokens, require independent approval for high-risk actions, and verify that repeated requests cannot bypass checks. Treat client-side JavaScript and hidden buttons as presentation, not policy.

### 6. Room questions

For each question, identify the intended business rule, the request that violates it, and the observable impact. Use the lab's scenario for the precise answer. State assumptions where the room does not provide enough evidence and redact public flags as `flag{hidden}`.
