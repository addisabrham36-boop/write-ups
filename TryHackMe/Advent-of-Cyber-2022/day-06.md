# Advent of Cyber 2022 - Day 6: Email Analysis

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Inspecting raw RFC 822 email message headers (`.eml` format) to detect spoofing, phishing, spam scores, and hidden tracking data.

## Walkthrough
Open `Urgent_Invoice.eml` in a text editor or email header analyzer:
```bash
cat Urgent_Invoice.eml | head -n 35
```
Key header findings:
- `From: Chief Elf <chief.elf@santaclaus.thm>`
- `Reply-To: murphy.evident@bandityeti.thm`
- `X-Spam-Score: 3`
- `Message-ID: <QW9DMjAyMl9FbWFpbF9BbmFseXNpcw==@santaclaus.thm>`

Decoding the Base64 portion of the Message-ID:
```bash
echo "QW9DMjAyMl9FbWFpbF9BbmFseXNpcw==" | base64 -d
# Output: AoC2022_Email_Analysis
```

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the email address of the sender? | `chief.elf@santaclaus.thm` |
| What is the return address? | `murphy.evident@bandityeti.thm` |
| On whose behalf was the email sent? | `Chief Elf` |
| What is the X-spam score? | `3` |
| What is hidden in the value of the Message-ID field? | `AoC2022_Email_Analysis` |
