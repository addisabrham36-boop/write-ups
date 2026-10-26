# Phishing Basics

{% embed url="https://tryhackme.com/room/phishingbasics" %}

A comprehensive exploration of phishing attack variants, social engineering psychological drivers, technical evasion methods (typosquatting), and defensive email authentication protocols.

---

## 1. Phishing Variants

- **Phishing:** Broad email-based social engineering attempting to harvest credentials or drop malware.
- **Smishing:** Phishing executed via SMS text messages.
- **Vishing:** Voice/phone call phishing.
- **Spear Phishing:** Highly targeted phishing aimed at a specific individual or organization.
- **Whaling:** Spear phishing targeting C-suite executives, board members, or high-profile public figures.

---

## 2. Psychological Triggers

Adversaries exploit universal human psychological cognitive biases:
- **Urgency:** Forcing hurried decisions with strict countdowns (e.g. "Account expires in 24 hours").
- **Authority:** Exploiting deference to hierarchy or law enforcement (e.g. CEO requesting gift cards or CFO demanding transfers).
- **Fear:** Exploiting panic (e.g. "Your account credentials were found in a dark web breach").
- **Curiosity / Greed:** Enticing victims with exclusive previews or financial gains.

---

## 3. Technical Defenses & Metrics

- **Typosquatting:** Registering domains that visually mimic authentic domains through common typing errors (e.g. `examp1e.com`).
- **Defensive Email Standards:** The triad defending domain authenticity:
  - **SPF:** Sender Policy Framework.
  - **DKIM:** DomainKeys Identified Mail.
  - **DMARC:** Domain-based Message Authentication, Reporting, and Conformance.
- **Campaign Metrics:**
  - An entry rate of 6% represents **High risk**.
  - **Attachment Detonation Rate:** Percentage of targets who download and open a weaponized attachment.

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | Primary channel used during a smishing attack | `SMS` |
| **Task 2** | Phishing email sent exclusively to a CEO | `Whaling` |
| **Task 3** | Principle: Offer for iPhone expiring in 24 hours | `Urgency` |
| **Task 3** | Principle: Executive requesting data emphasizing their position | `Authority` |
| **Task 3** | Principle: Promising exclusive access to an unknown product | `Curiosity` |
| **Task 3** | Principle: Account credentials found in a recent data breach | `Fear` |
| **Task 4** | Technique relying on users making a typo | `Typosquatting` |
| **Task 4** | Three security measures defending against email spoofing (Alphabetical) | `DKIM, DMARC, SPF` |
| **Task 5** | Risk level represented by a 6% credential entry rate | `High risk` |
| **Task 5** | Metric measuring percentage of users opening an attachment | `Attachment Detonation Rate` |
| **Task 5** | Single recommendation for a client with a 10% click rate | `Focused security awareness training` |
