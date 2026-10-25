# Phishing Basics: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/phishing

## Room Overview & Information

**Category:** Security awareness / email security  
**Difficulty:** Beginner  
**Focus:** Recognising phishing messages, inspecting sender details and links, understanding social engineering, and reporting suspicious email.

Phishing is a social-engineering attack that tricks someone into revealing information, opening a malicious attachment, approving a fraudulent action, or visiting a deceptive website. It often relies on urgency, fear, authority, curiosity, or a reward. A convincing message can imitate a real brand, so visual appearance alone is not proof of authenticity.

## Phase 1: Inspect the sender

Check the full sender address, not only the display name. A display name can be set to almost anything. Look for misspelled domains, extra words, unexpected subdomains, and a reply-to address that differs from the apparent sender.

For technical analysis, email headers can include:
- From and Reply-To.
- Return-Path.
- Received chain.
- Message-ID.
- Authentication results such as SPF, DKIM, and DMARC.

A header field can be spoofed, so assess the full header and authentication results rather than trusting a single line. SPF checks whether a sending server is authorised for a domain, DKIM verifies a signature over selected message data, and DMARC defines how a domain wants receivers to handle authentication failures and alignment.

## Phase 2: Examine the message content

Common warning signs include:
- Urgent threats or deadlines.
- Unexpected invoices, account warnings, or delivery notices.
- Requests for passwords, MFA codes, gift cards, or payment changes.
- Grammar or branding inconsistencies.
- Unexpected attachments.
- A link whose displayed text differs from its destination.
- A request to bypass normal approval or verification.

A legitimate message can contain typos, and a well-written message can still be malicious. Use several indicators together.

## Phase 3: Inspect links safely

Do not click an unexpected link just to see where it goes. Hover over it or copy the link address into a safe text editor to inspect the actual destination. Check the registered domain from right to left. For example, `login.example.com.attacker.test` belongs to `attacker.test`, not `example.com`.

Look for:
- Lookalike characters.
- Extra subdomains.
- URL shorteners.
- Encoded or misleading paths.
- A domain unrelated to the claimed organisation.
- HTTP instead of HTTPS where secure authentication is expected.

HTTPS means the connection is encrypted to the named host; it does not mean the host is trustworthy.

## Phase 4: Inspect attachments

Do not open unexpected attachments on a normal workstation. Check the sender through a separate trusted channel and follow organisational policy. Security teams may inspect file metadata, hashes, signatures, and sandbox reports in an isolated environment.

A file extension is not proof of file type. A document can contain active content, and an archive can contain nested files. Never upload confidential attachments to a public scanning service without approval.

## Phase 5: Respond to a suspected phish

If the message is suspicious:
1. Do not click the link or open the attachment.
2. Do not reply or provide credentials.
3. Report the message through the organisation's phishing-reporting process.
4. Preserve the message and headers if the security team requests them.
5. Verify unusual payment or account requests through a known, independent contact method.

If credentials were entered, notify the security team immediately, change the password through the legitimate site, revoke sessions where possible, and review MFA and account activity. If an attachment was opened, report that fact promptly; hiding it delays containment.

## Phase 6: Working through the room

For each example message, assess the sender, wording, URL destination, attachment, and requested action. Answer from the evidence presented in the room. If the task asks which clue makes a message suspicious, identify the specific field or behaviour rather than selecting an answer based only on the message's tone.

## Phase 7: Prevention

- Use MFA and phishing-resistant authentication where possible.
- Deploy email authentication and filtering.
- Block known malicious domains and attachments.
- Train users to report suspicious messages without fear of blame.
- Verify payment or bank-detail changes through a second channel.
- Use least privilege and limit the impact of a compromised account.
- Maintain a fast process for revoking sessions and resetting credentials.

## Key takeaways

Phishing succeeds by manipulating trust and urgency. Inspect the real sender and destination, validate unusual requests independently, avoid opening unexpected files, and report suspicious messages quickly. No single visual clue proves a message is safe or malicious; use the evidence as a whole.

## Expanded phishing-analysis workflow

### 1. Preserve the message

If the room provides an email, retain the original message or its raw source where possible. Record the sender, recipient, date, subject, Message-ID, attachment names, and the reported delivery path. Do not open unknown links or attachments on a personal device.

### 2. Check sender identity and authentication

Inspect the visible From address, Reply-To, Return-Path, and relevant mail headers. Display names can be spoofed and a legitimate-looking domain can contain subtle spelling changes. SPF, DKIM, and DMARC results provide useful context, but a pass result does not guarantee that a message is safe.

### 3. Inspect links safely

Read the actual URL destination rather than trusting the visible link text. Look for unexpected subdomains, misleading paths, URL shorteners, encoded destinations, and domains that resemble a legitimate brand. Do not visit a suspicious link unless the lab provides a safe detonation environment.

### 4. Analyse attachments

Check the filename, file type, hash, and sender context. File extensions can be misleading. Use the platform's sandbox or isolated analysis environment for suspicious attachments, and never enable macros or execute content merely to see what it does.

### 5. Look for social-engineering clues

Urgency, secrecy, requests to bypass normal approval, unusual payment details, unexpected password resets, and requests for credentials are warning signs. However, writing quality alone is not reliable: legitimate messages can be poorly written and phishing messages can be polished.

### 6. Report and contain

Use the organization's reporting mechanism. Preserve the message, block or quarantine it according to policy, search for similar messages, and reset credentials only when evidence suggests they were exposed. If a user clicked a link, determine whether they entered credentials, approved an MFA prompt, downloaded a file, or granted permissions.

### 7. Answer the room questions

For each task, point to the header, URL, attachment, or behavioural clue that supports the answer. Copy exact domains and sender fields from the provided sample. Do not claim that a message is malicious based on one clue alone; explain how the evidence combines. Redact public flags as `flag{hidden}`.
