# DFIR: An Introduction

{% embed url="https://tryhackme.com/room/dfiranintro" %}

An introduction to Digital Forensics and Incident Response (DFIR), forensic timeline generation, artifact extraction tools (Autopsy, KAPE, Volatility), and NIST/SANS incident lifecycle frameworks.

---

## 1. What is DFIR?

**Digital Forensics and Incident Response (DFIR)** combines two complementary security specializations:
1. **Digital Forensics:** The science of identifying, preserving, analyzing, and presenting digital evidence from disks, memory dumps, and network captures.
2. **Incident Response:** The operational process of containing, eradicating, and recovering from an active security breach.

---

## 2. Core Forensic Toolsets

- **Disk & File Analysis:** Autopsy, FTK Imager, KAPE.
- **Memory Forensics:** Volatility, Rekall.
- **Endpoint Triage:** Velociraptor, Redline.

---

## 3. The Incident Response Lifecycle

Comparing the standard SANS 6-step lifecycle to the NIST SP 800-61 framework:
- **Containment, Eradication, and Recovery:**
  - **Eradication:** Evicting the adversary from the network, closing vulnerabilities, and removing artifacts.
  - **Recovery:** Safely bringing compromised systems and services back online into production.
- **Post-Incident Activity:** The NIST equivalent to the SANS **"Lessons Learned"** stage.

---

## 4. Practical Timeline Exercise

Completing the log timeline reconstruction simulation provides the verification flag:
`THM{DFIR_REPORT_DONE}`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | What does DFIR stand for? | `Digital Forensics and Incident Response` |
| **Task 2** | DFIR requires expertise in Digital Forensics and what other field? | `Incident Response` |
| **Task 3** | Flag obtained from timeline creation exercise | `THM{DFIR_REPORT_DONE}` |
| **Task 5** | Stage where disrupted services are brought back online | `Recovery` |
| **Task 5** | Stage where threat is evicted after forensic analysis | `Eradication` |
| **Task 5** | NIST equivalent of SANS "Lessons learned" | `Post-incident Activity` |
