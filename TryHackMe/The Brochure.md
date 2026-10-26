# The Brochure

{% embed url="https://tryhackme.com/room/thebrochure" %}

An Open Source Intelligence (OSINT) and image analysis challenge from the Hacker Holidays event, tracking a fictional luxury hotel's digital footprint to extract a hidden social media flag.

---

## 1. Challenge Overview

- **Room:** The Brochure (The Byte Lotus Hotel)
- **Category:** OSINT / Digital Footprint Analysis
- **Difficulty:** Easy
- **Premise:** We are given a promotional brochure for the fictional "Byte Lotus Hotel". The challenge is to track down the online footprint of the business and its associated personnel.

---

## 2. Investigation Walkthrough

1. **Brochure Analysis:** Reviewing the marketing image for "The Byte Lotus Hotel", the prompt hints at AI-generated imagery and an active digital marketing trail.
2. **Social Media Reconnaissance:**
   - Search Instagram for profiles matching **"Byte Lotus Resorts"** or **"Byte Lotus Hotel"**.
   - Inspect the official hotel profile's **following** / **followers** list.
   - Spot the single individual account linked to the hotel staff (account profile *Vera*).
3. **Flag Extraction:**
   - Reviewing Vera's account posts and bio reveals an encoded Base64 string.
   - Decoding the string yields the victory flag.

```bash
echo "VEhNe...==" | base64 -d
```

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 1** | Target hotel name analyzed | `Byte Lotus Hotel` |
| **Task 1** | Primary platform where the staff footprint was uncovered | `Instagram` |
| **Task 1** | Flag format | Retrieved via Base64 decoding from the target profile bio (`THM{...}`) |
