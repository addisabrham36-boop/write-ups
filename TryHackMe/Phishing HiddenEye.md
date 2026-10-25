# Phishing with HiddenEye: TryHackMe Walkthrough

Room: https://tryhackme.com/room/hideye

This room concerns phishing simulation and the risks of fake login pages. A phishing kit may imitate a legitimate sign-in page to collect credentials, which is why it must only be used in a controlled training environment. Do not deploy a credential-harvesting page on a public host or send it to unsuspecting people.

## 1. Understand the scenario and scope

Read the room's instructions and identify the provided target environment. Use only the simulated accounts and local or lab network the exercise authorises. Never enter real passwords or tokens into a test page.

A tool's ability to generate a page does not make the page legitimate or the activity authorised. Confirm the permitted scope before launching any listener or sharing a test URL.

## 2. Understand how a phishing page works

A phishing page imitates a trusted service and persuades a user to submit information. A typical flow includes a lure, a link, a page that resembles a login form, and a server-side handler. The danger comes from collecting credentials or session material and then using them to access the real service.

In the lab, focus on understanding the page and its behaviour. Observe what form fields exist, where the form submits, and what the training environment records. Do not collect information from real users.

## 3. Recognise indicators

Defenders can identify phishing by examining:
- A domain that does not match the claimed organisation.
- Unexpected redirects or URL shorteners.
- Login forms hosted on unrelated infrastructure.
- Requests for credentials or MFA codes in an unusual context.
- Lookalike branding, urgency, or unexpected attachments.
- Suspicious page source, form action, or certificate details.

HTTPS does not prove that a page is genuine. Certificate validation protects the connection to the domain, not the user's judgement about whether the domain is trustworthy.

## 4. Safe lab workflow

Follow the room's exact setup instructions and keep the test listener within the authorised environment. Use the test accounts supplied by the lab, and verify the simulated result in the room's UI. Stop the service and remove any temporary artefacts when the exercise is complete.

If the tool fails, check whether the required dependencies and network interface are available in the lab. Do not expose a listener to the public internet simply to make the demonstration work.

## 5. Defensive controls

Use phishing-resistant MFA where possible, password managers that recognise the legitimate domain, email authentication and filtering, safe link handling, and user reporting. Organisations should monitor suspicious sign-in attempts and revoke sessions if credentials may have been exposed.

For a training simulation, use clear approval, limited test accounts, minimal data collection, and an agreed cleanup plan. The purpose is to measure and improve resilience, not to retain real credentials.

## 6. Working through the room

Use the supplied lab environment and follow each task's instructions. If the room asks what the tool does, explain the phishing workflow and the risk it demonstrates. If it asks for a value from the simulated page, use the result shown by the active room. Do not include real credentials or unverified flags in this note.

## What I took from this room

Phishing tools demonstrate how easily a convincing page can be created, but the security lesson is about trust and verification. Use simulations responsibly, protect credentials, and validate the domain before entering sensitive information.