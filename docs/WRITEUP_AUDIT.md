# Write-Up Audit Checklist

**Repository:** `addisabrham36-boop/write-ups`  
**Branch:** `main`  
**Audit started:** 2026-10-26  
**Last updated:** 2026-10-26

---

## Summary

| Metric | Count |
|---|---|
| Total files discovered | 91 (90 TryHackMe + 1 PortSwigger) |
| Files with Q&A section | ~25 |
| Files needing expansion | ~65 |

---

## TryHackMe — CTF Machines (COMPLETE)

| Room | Status | Verified Answers |
|---|---|---|
| Basic Pentesting | ✅ COMPLETE | jan:armando, beeswax, flags |
| Blue (EternalBlue) | ✅ COMPLETE | 3 flags, alqfna22 |
| Bypass Functions | ✅ COMPLETE | LD_PRELOAD / Chankro chain |
| Crack the Hash | ✅ COMPLETE | All Level 1 & 2 |
| Cyber Heroes | ✅ COMPLETE | flag{edb0be532c540b1a...} |
| Gaming Server | ✅ COMPLETE | john:letmein, LXD flags |
| Guided Pentest Infrastructure | ✅ COMPLETE | UnrealIRCd backdoor |
| Guided Pentest Web | ✅ COMPLETE | IDOR → RCE chain |
| Hydra | ✅ COMPLETE | THM{2673a...}, THM{c8eeb...} |
| Internal | ✅ COMPLETE | admin:my210473, both THM flags |
| MD2PDF | ✅ COMPLETE | SSRF → localhost:5000 |
| Mr Robot CTF | ✅ COMPLETE | 3 keys verified |
| Ninja Skills | ✅ COMPLETE | D8B3 v2Vb, c4ZX, bny0, X1Uy, 8V2L |
| No Scope Finding RCE | ✅ COMPLETE | CVE-2026-35482 |
| Oh My WebServer | ✅ COMPLETE | CVE-2021-41773 |
| OhSINT | ✅ COMPLETE | All 7 verified |
| OverlayFS CVE-2021-3493 | ✅ COMPLETE | Kernel 5.4.0-72 |
| Phantom Fob | ✅ COMPLETE | XOR auth formula |
| PHP 2 Shell | ✅ COMPLETE | session.upload_progress chain |
| Psycho Break | ✅ COMPLETE | joseph:intotheterror445, both flags |
| Python Playground | ✅ COMPLETE | Sandbox escape + Docker |
| Road | ✅ COMPLETE | BahamasChapp123!@#, both hashes |
| TryHack3M Bricks Heist | ✅ COMPLETE | THM{fl46_650c...}, LockBit |
| Vulnversity | ✅ COMPLETE | phtml bypass, both flags |
| Airplane | ✅ COMPLETE | gdbserver, ruby sudo |

---

## TryHackMe — Theory Rooms (COMPLETE)

| Room | Status | Notes |
|---|---|---|
| Active Reconnaissance | ✅ COMPLETE | All Q&A |
| Extracted Enum & Brute Force | ✅ COMPLETE | All Q&A |
| Introduction to SIEM | ✅ COMPLETE | cudominer.exe, Event 104 |
| Linux Fundamentals Part 1 | ✅ COMPLETE | THM{ACCESS} |
| Nmap Live Host Discovery | ✅ COMPLETE | ARP sim answers |
| Passive Reconnaissance | ✅ COMPLETE | THM{a5b83929...}, all answers |

---

## IN PROGRESS — Need Q&A Sections

| Room | Lines | Priority | Next Action |
|---|---|---|---|
| Snort | 195 | HIGH | Add Q&A table with rule syntax answers |
| HTTP in Detail | 255 | HIGH | Add Q&A for all tasks |
| DNS in Detail | 155 | HIGH | Add Q&A for all tasks |
| Metasploit Introduction | 150 | HIGH | Add Q&A |
| Google Dorking | 139 | HIGH | Add Q&A |
| Windows Basics | 163 | HIGH | Add Q&A |
| Advanced SQL Injection | 211 | HIGH | Add Q&A |
| Intro to LAN | 188 | MED | Add Q&A |
| What Is Networking | 163 | MED | Add Q&A |
| Offensive Security Intro | 59 | MED | Add Q&A |
| OWASP A01 Broken Access | 60 | MED | Add Q&A |
| OWASP Security Misconfig | 57 | MED | Add Q&A |
| OWASP Injection | ~120 | MED | Add Q&A |
| OWASP Insecure Data | 142 | MED | Add Q&A |
| OWASP App Design Flaws | 148 | MED | Add Q&A |
| OWASP IAAA Failures | 160 | MED | Add Q&A |
| Cypheron | 138 | MED | Full rewrite needed |
| Phishing Basics | 74 | MED | Add Q&A |
| Phishing HiddenEye | 81 | LOW | Add Q&A |
| Phishing Merry Clickmas | 130 | LOW | Add Q&A |
| AI Forensics | ~100 | LOW | Add Q&A |
| AI Models and Data | 66 | LOW | Add Q&A |
| AI Security Threats | 226 | LOW | Add Q&A |
| AI System Recon | 83 | LOW | Add Q&A |
| AI Threat Modelling | ~90 | LOW | Add Q&A |
| Android Hacking 101 | 128 | LOW | Add Q&A |
| Android Malware Analysis | 117 | LOW | Add Q&A |
| Careers in Cyber | 103 | LOW | Add Q&A |
| Cryptography for Dummies | 138 | LOW | Add Q&A |
| Data Encoding | 138 | LOW | Add Q&A |
| Defensive Security Intro | ~100 | LOW | Add Q&A |
| DFIR An Introduction | ~90 | LOW | Add Q&A |
| Inside a Computer System | 118 | LOW | Add Q&A |
| Intro to Cryptography | ~100 | LOW | Add Q&A |
| Intro to Operating Systems | 160 | LOW | Add Q&A |
| Junior SOC Analyst Intro | ~90 | LOW | Add Q&A |
| LLM Security | ~100 | LOW | Add Q&A |
| Malware Introductory | 108 | LOW | Add Q&A |
| Malware Researching | 110 | LOW | Add Q&A |
| Networking Concepts | 126 | LOW | Add Q&A |
| Operating Systems | 145 | LOW | Add Q&A |
| Passive Recon Notes | ~80 | LOW | Merge into Passive Recon |
| Pentesting Fundamentals | ~100 | LOW | Add Q&A |
| Prompt Engineering | ~90 | LOW | Add Q&A |
| Python Core Concepts | 165 | LOW | Add Q&A |
| SDLC | 72 | LOW | Add Q&A |
| Search Skills | 133 | LOW | Add Q&A |
| Securing AI Systems | 102 | LOW | Add Q&A |
| SOC Role in Blue Team | 66 | LOW | Add Q&A |
| The Brochure | ~100 | LOW | Add Q&A |
| The Building Blocks of AI | 137 | LOW | Add Q&A |
| The Hacker Methodology | 112 | LOW | Add Q&A |
| THM Fundamentals | 275 | LOW | Add Q&A |
| Wi-Fi Fundamentals & Recon | 125 | LOW | Add Q&A |
| WiFi Fundamentals | ~90 | LOW | Add Q&A |
| WiFi Hacking | 83 | LOW | Add Q&A |

---

## Advent of Cyber (IN PROGRESS)

| Room | Lines | Status | Notes |
|---|---|---|---|
| Advent of Cyber 2022 | 725 | IN PROGRESS | Needs per-day Q&A |
| Advent of Cyber 2023 | 603 | IN PROGRESS | Needs per-day Q&A |
| Advent of Cyber Prep Track | 326 | IN PROGRESS | Needs Q&A |

---

## PortSwigger (IN PROGRESS)

| Room | Lines | Status |
|---|---|---|
| API Test | ~50 | IN PROGRESS |
