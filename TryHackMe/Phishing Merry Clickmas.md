# Phishing: Merry Clickmas — TryHackMe

Room: https://tryhackme.com/room/phishingmerryclickmas

This room focuses on identifying phishing messages during a holiday-themed scenario. Attackers often use seasonal events, delivery notices, gift offers, and urgent account warnings because people are busy and more likely to act quickly.

## Inspect the message

I should check the sender address rather than relying on the display name, inspect where links actually point, and pay attention to unexpected attachments. A link's visible text can differ from its destination. Urgency, unusual requests, and requests for credentials are additional warning signs, but no single clue proves a message is malicious.

If the room provides a sample email, I should inspect the message and headers using the provided tools. I should not click a suspicious link on my personal device just to test it.

## Email authentication

SPF, DKIM, and DMARC provide signals about whether a message is authorised and aligned with the claimed domain. Their presence is useful, but authentication results do not guarantee that the message is safe. A compromised legitimate account can still send phishing messages.

## What I took from this room

Phishing analysis combines visual clues, link inspection, sender information, and technical evidence. The safest habit is to pause before acting, verify unexpected requests through a separate trusted channel, and report suspicious messages through the approved process.