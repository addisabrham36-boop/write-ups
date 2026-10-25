# Phishing Basics — TryHackMe

Room: https://tryhackme.com/room/phishingemails

Phishing is a social-engineering attack where someone tries to trick a person into revealing information, opening a malicious file, or visiting a harmful site. It works by exploiting trust and urgency rather than relying only on a technical vulnerability.

## What I check in an email

I look at the sender address and display name, the actual destination of links, the wording, unexpected attachments, and whether the message pressures the recipient to act quickly. A familiar logo or a convincing layout is not proof that a message is legitimate.

Email headers can provide more technical context, such as the servers involved in delivery and authentication results. SPF, DKIM, and DMARC help receiving systems evaluate aspects of sender legitimacy, but none of them guarantees that the message content is safe.

## Handling suspicious messages

I should not click a suspicious link just to see what happens on a normal device. In an organisation, the right process is to report the message using the approved channel and let the security team investigate it. In a training room, I should use the provided sample and tools.

## What I took from this room

Phishing detection means checking multiple clues together. A single typo does not prove a message is malicious, and polished writing does not prove it is genuine. I need to inspect the sender, links, context, and technical evidence before making a decision.