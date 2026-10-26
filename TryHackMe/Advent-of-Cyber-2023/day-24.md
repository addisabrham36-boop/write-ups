# Advent of Cyber 2023 - Day 24: Mobile Forensics

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Digital forensics on an Android mobile device image using **Autopsy**.

## Walkthrough
1. Load the mobile physical image into Autopsy.
2. Ingest modules: Android Analyzer, Picture Gallery, Communications.
3. Review extracted artifacts:
   - Image Gallery: Inspecting holiday photos reveals a flag overlay: `THM{DIGITAL_FORENSICS}`.
   - Contact List: Detective Frost-eau is saved under contact alias: `Detective Carrot-Nose`.
   - SMS / Messaging Database: Message conversation with Van Sprinkles exposes password: `chee7AQu`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Flag hidden inside one of the photos? | `THM{DIGITAL_FORENSICS}` |
| Name Tracy used to save Detective Frost-eau's phone number? | `Detective Carrot-Nose` |
| Password inside SMS exchanged with Van Sprinkles? | `chee7AQu` |
