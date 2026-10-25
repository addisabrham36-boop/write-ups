# Phishing Basics: TryHackMe Walkthrough

Room: https://tryhackme.com/room/phishing

Phishing is a social-engineering technique that tricks a person into revealing information, opening a malicious file, approving an action, or visiting a harmful website. It often uses urgency, impersonation, familiar branding, or an unexpected request to make the victim act before checking the evidence.

## 1. Recognise common forms

- **Email phishing:** a message impersonates a trusted sender or service.
- **Spear phishing:** a message is tailored to a particular person or organisation.
- **Smishing:** phishing delivered through SMS or messaging apps.
- **Vishing:** voice-based social engineering.
- **Business email compromise:** an attacker abuses or impersonates a business account to influence payments or sensitive actions.
- **Credential harvesting:** a fake login page attempts to collect passwords or session details.

The channel and the goal are separate. A text message that directs someone to a fake login page is still a phishing attempt.

## 2. Inspect the message

Do not rely only on the display name. Check the actual sender address, reply-to address, link destination, language, timing, and request. A message may contain a legitimate-looking logo while the underlying domain is unrelated.

Hovering over a link can reveal its destination without opening it, but do not click a suspicious link merely to inspect it. On an authorised training sample, inspect the URL and identify the registered domain, subdomain, path, and unusual spelling. HTTPS alone does not prove that a site is trustworthy.

## 3. Read email headers

Email headers can reveal the sending infrastructure and the route taken by the message. Useful fields include From, Reply-To, Return-Path, Received, Message-ID, and authentication results. Multiple Received headers are added as mail servers handle the message, and should be read in the correct order.

SPF checks whether a sending server is authorised by a domain's policy. DKIM validates a cryptographic signature associated with a domain. DMARC specifies how a receiver should handle messages that fail alignment checks and can provide reporting. These controls help, but a compromised legitimate account can still send malicious mail.

## 4. Examine links and attachments safely

If a message contains an attachment, do not open it on a normal workstation just to see what it does. In a lab, follow the supplied analysis process. Record the file name, type, size, hash, and relevant metadata. If malware analysis is required, use an isolated environment and avoid connecting it to personal accounts or sensitive networks.

For a URL, record the hostname and path, and use approved reputation or sandbox tools where appropriate. Reputation services can miss new threats and may receive the URL or file you submit, so consider privacy before uploading anything.

## 5. Make a decision and report it

A useful triage note includes:
- Why the message is suspicious.
- The sender and destination domains.
- Relevant header or authentication evidence.
- Any attachment metadata or URL indicators.
- Whether a user interacted with the message.
- Recommended containment and reporting steps.

Do not mark every unfamiliar email as malicious. Legitimate messages can be unexpected, and phishing messages can be carefully written. Use multiple signals and verify sensitive requests through a separate, trusted channel.

## 6. What to do if someone clicked

Follow the organisation's incident process. Report the message, preserve relevant details, and assess whether credentials were entered or a file executed. If credentials may be exposed, reset them through the legitimate service and revoke sessions where appropriate. If a file ran, involve the security team for endpoint investigation. Do not delete evidence before the response team has had a chance to collect it.

## 7. Working through the room

For each sample message, identify the suspicious indicators and the action the sender wants the recipient to take. If the question asks for a URL or domain, use the actual value shown in the room. Explain why the message is suspicious instead of guessing from the subject line alone.

## What I took from this room

Phishing analysis is about validating identity, destination, and intent. The safest response is to pause, inspect the evidence, verify unusual requests independently, and report suspicious messages through the proper channel.