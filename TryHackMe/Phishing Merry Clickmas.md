# Phishing: Merry Clickmas — TryHackMe Walkthrough

Room: https://tryhackme.com/room/phishingmerryclickmas

This room uses a seasonal phishing scenario to practise recognising suspicious messages. The festive theme is decoration; the analysis process is the same as for any email: establish who sent it, where links lead, what attachments contain, and what action the sender wants from the recipient.

## 1. Read the message as evidence

Start with the sender address, display name, subject, greeting, requested action, and any deadline or threat. Look for a mismatch between the claimed organisation and the actual sending domain. Urgency is a warning sign, but it is not proof on its own.

Ask what the sender wants the recipient to do: enter credentials, open an attachment, approve a payment, change account settings, or share private information. This helps identify the likely goal of the campaign.

## 2. Inspect the link destination

A displayed link can hide a different destination. In a safe training environment, inspect the URL without blindly following it. Check the registered domain, spelling, subdomains, path, and whether the link uses an unexpected redirect or URL shortener.

A lock icon or HTTPS connection only indicates that the connection is encrypted to the site presented. It does not prove that the site belongs to the organisation named in the message.

## 3. Examine sender and authentication details

Where the room provides email headers, inspect the actual From address, Reply-To, Return-Path, Received chain, and SPF/DKIM/DMARC results. These can help identify spoofing or unexpected sending infrastructure.

Authentication results need context. A message can pass authentication for a malicious domain, and an attacker may abuse a compromised legitimate account. Do not treat a single passing check as a guarantee of safety.

## 4. Review attachments cautiously

Do not execute unknown attachments on your main system. In the lab, use the provided safe analysis workflow and inspect metadata before opening a file. Record the file type and hash where the task asks for them. If a sandbox is used, keep it isolated and do not enter real credentials.

A filename extension is not proof of the true file type. A document may also contain links, macros, embedded objects, or instructions that attempt to persuade the user to enable risky features.

## 5. Decide whether to report

Use a combination of evidence: sender mismatch, unexpected link, suspicious attachment, unusual request, failed authentication alignment, or a known malicious indicator. If a message requests a payment or credential change, verify it through a known contact method rather than replying to the message.

A clear report states the suspicious indicators, the target URL or attachment metadata, whether anyone interacted with it, and the next action. Preserve the original message for the security team.

## 6. Working through the room

Read each email sample carefully and follow the task's question. If asked to identify a phishing clue, point to the exact header, URL, attachment, or request in the sample. If asked whether a message is malicious, base the decision on multiple signals and the room's evidence.

The live exercise may use specific sample details that are not included here, so do not copy guessed domains or answers from another walkthrough. Verify the current sample and use the room's feedback.

## What I took from this room

Phishing is successful when a message causes a person to skip verification. Slow down, inspect the true sender and destination, avoid opening unknown attachments, and confirm sensitive requests through a separate trusted channel.