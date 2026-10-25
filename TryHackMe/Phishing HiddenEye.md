# Phishing with HiddenEye: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/hideye

## Room Overview & Information

**Category:** Phishing simulation / social engineering  
**Difficulty:** Beginner  
**Focus:** Understanding fake login pages, how credential-harvesting kits work, and how to identify and defend against phishing.

A phishing kit can imitate a legitimate sign-in page and persuade a user to submit credentials. This is why the tool must be used only in a controlled training environment. Do not deploy a credential-harvesting page on a public host, send it to unsuspecting people, or collect real credentials.

## Phase 1: Understand the scope

Read the room's instructions and identify the provided target environment. Use only the simulated accounts and local or lab network the exercise authorises. Never enter real passwords, API keys, or session tokens into a test page.

A tool's ability to generate a page does not make the page legitimate or the activity authorised. Confirm the permitted scope before launching any listener or sharing a test URL.

## Phase 2: Understand the phishing workflow

A phishing page generally has four parts:
1. **Lure:** a message or situation persuades a user to visit the page.
2. **Lookalike page:** the page imitates a trusted service.
3. **Submission handler:** form data is sent to a backend.
4. **Follow-up:** the user may be redirected or shown a message after submitting.

The risk is not limited to the appearance of the page. The backend may collect credentials or session material and make it available to an attacker. The page can also redirect a user to the real service to hide what happened.

In the lab, focus on understanding the page and its behaviour. Observe which fields exist, where the form submits, and what the training environment records. Do not collect information from real users.

## Phase 3: Inspect the page and its destination

When reviewing a suspected phishing page, inspect:
- The full domain and registered domain.
- TLS certificate details and hostname match.
- Form action and request destination.
- JavaScript and redirects.
- Whether the page requests an MFA code or other sensitive information.
- Whether the page's branding and workflow match the real service.

A valid HTTPS certificate does not prove that a page is genuine. It only helps establish an encrypted connection to the domain in the address bar.

## Phase 4: Recognise indicators

Common phishing indicators include:
- A domain that does not match the claimed organisation.
- Unexpected redirects or URL shorteners.
- Login forms hosted on unrelated infrastructure.
- Requests for passwords or MFA codes in an unusual context.
- Lookalike branding, urgency, or unexpected attachments.
- A suspicious form action in the page source.
- A newly registered or unrelated domain used for a well-known brand.

No single indicator is conclusive. A legitimate service may use a third-party identity provider, and a malicious page can be visually convincing. Correlate the domain, message context, and request destination.

## Phase 5: Safe lab workflow

Follow the room's exact setup instructions and keep any listener within the authorised environment. Use only the test accounts supplied by the lab, and verify the simulated result in the room's UI. Stop the service and remove temporary artefacts when the exercise is complete.

If the tool fails, check whether the required dependencies and network interface are available in the lab. Do not expose a listener to the public internet simply to make the demonstration work, and do not attempt to bypass a real service's controls.

## Phase 6: Incident response if credentials are submitted

If a user submits credentials to a suspected phishing page:
1. Notify the security team immediately.
2. Change the password through the legitimate service.
3. Revoke active sessions and refresh tokens where possible.
4. Review MFA methods and recovery details.
5. Check recent sign-ins, mailbox rules, and account changes.
6. Rotate other credentials if the same password was reused.
7. Preserve the message, URL, headers, and relevant logs.

Do not blame the user. Fast reporting helps reduce impact and provides evidence for investigation.

## Phase 7: Defensive controls

- Use phishing-resistant MFA where possible.
- Use a password manager that recognises the legitimate domain.
- Deploy email authentication, filtering, and safe-link analysis.
- Monitor for suspicious authentication attempts.
- Restrict access to credential collection and identity administration.
- Provide a simple reporting process and clear user guidance.
- For approved simulations, use dedicated test accounts, minimal data collection, and a documented cleanup plan.

## Phase 8: Working through the room

Use the supplied lab environment and follow each task's instructions. If a question asks what the tool demonstrates, explain the phishing workflow and the risk of credential collection. If it asks for a value from the simulated page, use the result shown by the active room. Do not include real credentials or unverified flags in this note.

## Key takeaways

Phishing tools demonstrate how easily a convincing page can be created, but the security lesson is about trust and verification. Use simulations responsibly, protect credentials, validate the domain before entering sensitive information, and respond quickly if credentials may have been exposed.