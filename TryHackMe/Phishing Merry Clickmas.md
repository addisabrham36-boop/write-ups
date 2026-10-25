# Phishing: Merry Clickmas — TryHackMe Walkthrough

Official room: https://tryhackme.com/room/phishingmerryclickmas

## Room Overview & Information

**Category:** Email security / phishing analysis  
**Difficulty:** Beginner  
**Focus:** Identifying suspicious senders, analysing message content, inspecting URLs and attachments, and deciding how to respond.

The room uses a seasonal phishing scenario to practise recognising suspicious messages. The festive theme is decoration; the analysis process is the same as for any email. Establish who sent it, where links lead, what attachments contain, and what action the sender wants from the recipient.

## Phase 1: Read the message as evidence

Start with the sender address, display name, subject, greeting, requested action, and any deadline or threat. Look for a mismatch between the claimed organisation and the actual sending domain. Urgency is a warning sign, but it is not proof by itself.

Ask what the sender wants the recipient to do: enter credentials, open an attachment, approve a payment, change account settings, or share private information. The requested action often reveals the likely goal of the campaign.

Do not decide solely from grammar or visual design. A well-written message can be malicious, and a legitimate message can contain spelling mistakes. Use multiple indicators.

## Phase 2: Inspect the link destination

A displayed link can hide a different destination. In a safe training environment, hover over it or inspect the address without blindly following it. Check the registered domain, spelling, subdomains, path, and whether the link uses an unexpected redirect or URL shortener.

For example, `accounts.example.com.attacker.test` belongs to `attacker.test`, not `example.com`. Read domains from right to left and identify the actual registered domain.

A lock icon or HTTPS connection only indicates that the connection is encrypted to the host presented. It does not prove that the host belongs to the organisation named in the message.

## Phase 3: Examine sender and authentication details

Where the room provides email headers, inspect the actual From address, Reply-To, Return-Path, Received chain, Message-ID, and SPF/DKIM/DMARC results.

- **SPF** checks whether a sending server is authorised for a domain.
- **DKIM** verifies a cryptographic signature over selected message data.
- **DMARC** defines policy and alignment expectations for a domain's authentication results.

These checks need context. A message can pass authentication for a malicious domain, and an attacker may abuse a compromised legitimate account. Do not treat a single passing check as a guarantee of safety.

## Phase 4: Review attachments cautiously

Do not execute unknown attachments on your main system. In the lab, use the provided safe analysis workflow and inspect metadata before opening a file. Record the file type and hash where the task asks for them. If a sandbox is used, keep it isolated and never enter real credentials.

A filename extension is not proof of the true file type. A document may contain links, macros, embedded objects, or instructions that try to persuade the user to enable risky features. Archives may contain nested files that need to be inspected separately.

## Phase 5: Evaluate the requested action

Phishing messages often create pressure by claiming that an account will be locked, a parcel will be returned, a payment is overdue, or a reward will expire. The user is encouraged to act before verifying the request.

For a message that asks for credentials, go to the service through a known bookmark or manually entered address instead of using the link. For a payment or bank-detail change, verify through a separate known contact method. Do not use the phone number or reply address supplied by the suspicious message as the only verification channel.

## Phase 6: Decide whether to report

Use a combination of evidence: sender mismatch, unexpected link, suspicious attachment, unusual request, failed authentication alignment, or a known malicious indicator. If the message is suspicious, report it through the organisation's process and preserve the original email and headers.

A useful report states:
- Why the message appears suspicious.
- The sender and destination domain.
- The attachment name and hash, if available.
- Whether anyone clicked the link or opened the file.
- Whether credentials or payment details were entered.
- The time the message arrived and any related activity.

If credentials were entered, notify the security team immediately, change the password through the legitimate site, revoke sessions where possible, and review MFA and account activity. If an attachment was opened, report that fact promptly; hiding it delays containment.

## Phase 7: Working through the room

Read each sample carefully and follow the task's question. If asked to identify a phishing clue, point to the exact header, URL, attachment, or request in the sample. If asked whether a message is malicious, base the decision on multiple signals and the room's evidence.

The live exercise may use specific sample details that are not included here, so do not copy guessed domains or answers from another walkthrough. Verify the current sample and use the room's feedback.

## Phase 8: Prevention

- Use phishing-resistant MFA where possible.
- Deploy email authentication and filtering.
- Block known malicious domains and attachments.
- Train users to report suspicious messages without fear of blame.
- Verify payment or account changes through a second channel.
- Use least privilege to limit the impact of compromised accounts.
- Maintain a rapid process for revoking sessions and resetting credentials.
- Monitor suspicious sign-ins and impossible-travel patterns.

## Key takeaways

Phishing succeeds when a message causes a person to skip verification. Inspect the true sender and destination, avoid opening unexpected attachments, and confirm sensitive requests through a separate trusted channel. No single visual clue proves a message is safe or malicious; evaluate the evidence as a whole.