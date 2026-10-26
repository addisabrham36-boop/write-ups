# Phishing Basics

Official room: https://tryhackme.com/room/phishing

## Room Overview & Information

**Category:** Security awareness and email security  
**Difficulty:** Beginner  
**Focus:** Recognising phishing messages, inspecting sender details and links, understanding social engineering, and reporting suspicious email.

Phishing tricks someone into revealing information, opening a malicious attachment, approving a fraudulent action, or visiting a deceptive website. It often relies on urgency, fear, authority, curiosity, or a reward. A convincing message can imitate a real brand, so appearance alone is not proof of authenticity.

This is an awareness and analysis room, not a boot-to-root target. Port scanning, initial access, and privilege escalation do not apply to the basic email examples.

## Phase 1: Reconnaissance & Sender Enumeration

### 1. Inspect the sender

Check the full sender address, not only the display name. A display name can be set to almost anything. Look for misspelled domains, extra words, unexpected subdomains, and a reply-to address that differs from the apparent sender.

Email headers can include:
- From and Reply-To.
- Return-Path.
- Received chain.
- Message-ID.
- Authentication results such as SPF, DKIM, and DMARC.

A header field can be spoofed, so assess the full header and authentication results rather than trusting a single line. SPF checks whether a sending server is authorised for a domain; DKIM verifies a message signature; and DMARC defines how receivers should handle authentication failures and alignment.

### 2. Preserve the evidence

If the room provides an email, retain the original message or its raw source where possible. Record the sender, recipient, date, subject, Message-ID, attachment names, and delivery path. Do not open unknown links or attachments on a personal device.

## Phase 2: Vulnerability Analysis & Link/Attachment Inspection

### 3. Examine message content

Warning signs can include urgent threats or deadlines, unexpected invoices, account warnings, delivery notices, requests for passwords or MFA codes, payment changes, unexpected attachments, and links whose visible text differs from their destination.

A legitimate message can contain typos, and a polished message can still be malicious. Evaluate several indicators together rather than relying on grammar alone.

### 4. Inspect links safely

Do not click an unexpected link just to see where it goes. Hover over it or inspect the actual link address in a safe text editor. Check the registered domain from right to left.

For example, the hostname login.example.com.attacker.test belongs to attacker.test, not example.com. Look for lookalike characters, extra subdomains, URL shorteners, encoded paths, redirects, and domains unrelated to the claimed organisation.

HTTPS means the connection is encrypted to the named host. It does not mean the host is trustworthy or belongs to the organisation being impersonated.

### 5. Inspect attachments carefully

Do not open unexpected attachments on a normal workstation. Verify the sender through a separate trusted channel and follow organisational policy. Security teams may inspect file metadata, hashes, signatures, and sandbox reports in an isolated environment.

A file extension is not proof of file type. A document can contain active content, and an archive can contain nested files. Never upload confidential attachments to a public scanning service without approval.

## Phase 3: Response, Containment & Post-Incident Checks

### 6. Respond to a suspected phish

1. Do not click the link or open the attachment.
2. Do not reply or provide credentials.
3. Report the message through the organisation's phishing-reporting process.
4. Preserve the message and headers if the security team requests them.
5. Verify unusual payment or account requests through a known, independent contact method.

If credentials were entered, notify the security team promptly, change the password through the legitimate site, revoke sessions where possible, and review MFA and account activity. If an attachment was opened, report that fact quickly so containment can begin.

### 7. Identify evidence of impact

A phishing investigation may examine email headers, URL redirects, DNS and web-server logs, authentication logs, mailbox rules, session grants, and endpoint activity after a click. A link being opened does not prove credentials were stolen. Establish whether the user submitted credentials, approved an MFA prompt, downloaded a file, or granted permissions.

## Phase 4: Room Questions & Task Walkthrough

For each example message, assess the sender, wording, URL destination, attachment, and requested action. If the task asks which clue makes a message suspicious, identify the specific field or behaviour rather than selecting an answer based only on tone. The room's examples and feedback are the authority for any interactive response.
