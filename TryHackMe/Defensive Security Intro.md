# Defensive Security Intro

{% embed url="https://tryhackme.com/room/defensivesecurityintro" %}

An introduction to the role of defensive security, SOC operations, DFIR (Digital Forensics & Incident Response), malware classifications, and hands-on threat blocking.

---

## 1. What is Defensive Security?

Defensive security focuses on preventing, detecting, and mitigating attacks. In security terminology, defensive practitioners belong to the **Blue Team**.

Key defensive pillars:
- **Security Operations Centre (SOC):** Team of security engineers and analysts monitoring networks and endpoints around the clock.
- **DFIR:** Digital Forensics and Incident Response — collecting digital evidence, establishing breach timelines, and eradicating threats.
- **Ransomware:** Destructive malware that encrypts victim files and extorts cryptocurrency payments in exchange for decryption keys.

---

## 2. Practical Defensive Threat Blocking

In the interactive banking defense simulation:
1. Review the real-time alert feed for unauthorized external authentication attempts.
2. Verify the suspicious external IP address.
3. Apply a perimeter firewall drop rule to isolate the attacker.
4. The simulation completes and returns the victory flag:
   `flag{hidden}`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 1** | Which team focuses on defensive security? | `Blue Team` |
| **Task 2** | Team of cybersecurity professionals monitoring systems for malicious events | `Security Operations Centre` |
| **Task 2** | What does DFIR stand for? | `Digital Forensics and Incident Response` |
| **Task 2** | Kind of malware requiring money to regain file access | `Ransomware` |
| **Task 3** | Flag obtained by blocking the active threat in the lab | `flag{hidden}` |
