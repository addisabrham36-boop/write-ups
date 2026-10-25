# Technical Write-Ups 

Welcome to my central documentation repository. This repository acts as a public portfolio of my technical write-ups, security room analyses, and engineering walkthroughs. 

My goal here is to document my problem solving methodology, explain complex technical configurations, and archive my hands-on experience cleanly.

---

## 📂 Repository Structure

The write-ups are organized logically by platform or topic:

*   📂 `/PortSwigger-Web-Security` — Lab walkthroughs focusing on API security and web vulnerabilities.
*   📂 `/TryHackMe` — Room solutions covering penetration testing, network analysis, and Linux fundamentals.
*   📂 `/Projects` — Step-by-step documentation for personal homelabs and security engineering builds.
*   📂 `/hack the box` — ctf and room solutions and walkthroughs.
*   📂 `/overthewire` - i will do all of them from bandit to formula one and write the walkthrough here.
*   📂 `/kc7` - most peoples are not familiar with this if you are familiar good other wise its good to learn blue teaming and KQL.and i will do writeups for the challenges.

---

## 🚩 TryHackMe

| Room / Writeup | Category | Topics / Techniques |
|:---|:---:|:---|
| [TryHackMe Fundamentals: Networking, Lan essentials & Linux](./TryHackMe/TryHackMe%20Fundamentals%3A%20Networking%2C%20Lan%20essentials%20%26%20Linux.md) | Fundamentals | Linux FHS, SUID, permissions, OSI vs TCP/IP, ARP, DHCP, Nmap, Wireshark |
| [TryhackMe OWASP Top 10 2025: Room IAAA Failures](./TryHackMe/TryhackMe%20OWASP%20Top%2010%202025.%20Room%3A%20IAAA%20Failures.md) | Web Security | OWASP Top 10, IDOR, Broken Access Control, Authentication Failures |
| [OhSINT Try Hack Me ROOM:](./TryHackMe/OhSINT%20Try%20Hack%20Me%20ROOM:.md) | OSINT | Reconnaissance, Social Media Intelligence, EXIF metadata analysis |
| [Vulnversity TryHackMe](./TryHackMe/Vulnversity%20%20TryHackMe.md) | Web & Privesc | Reconnaissance, File Upload Extension Bypass, SUID systemctl exploit |
| [Blue TryHackMe](./TryHackMe/Blue%20TryHackMe%20.md) | Network Exploitation | EternalBlue (MS17-010), Metasploit, Post-exploitation, Hash dumping |
| [TryHackMe AIRPLANE](./TryHackMe/TryHackMe%20AIRPLANE%20.md) | Black Box CTF | Directory fuzzing, Local File Inclusion (LFI), gdb sudo privilege escalation |
| [TryHackMe MR.Robot](./TryHackMe/TryHackMe%20MR.Robot%20%20.md) | Web & Privesc | Robots.txt discovery, WordPress brute force, Theme Editor RCE, SUID nmap |
| [Tryhackme : python playground walkthrough](./TryHackMe/Tryhackme%20%20%3A%20python%20playground%20walkthrough.md) | Hard CTF | Client-side JS bypass, Python jailbreak, Shared mount SUID escape |

---

## 🌐 PortSwigger Web Security

| Lab / Topic | Category | Description |
|:---|:---:|:---|
| [API Test](./PortSwigger-Web-Security/API%20Test.md) | API Security | API Recon, Exploiting endpoints via documentation, Hidden parameter discovery |


## Additional TryHackMe Notes

These pages cover fundamentals, defensive security, web testing, AI security, forensics, and CTF practice. Some are concept notes rather than verified end-to-end machine walkthroughs; I add target-specific commands and results only when they have been checked against the active room.

### Fundamentals and networking
- [What Is Networking?](./TryHackMe/What%20Is%20Networking.md)
- [Intro to LAN](./TryHackMe/Intro%20to%20LAN.md)
- [Networking Concepts](./TryHackMe/Networking%20Concepts.md)
- [DNS in Detail](./TryHackMe/DNS%20in%20Detail.md)
- [HTTP in Detail](./TryHackMe/HTTP%20in%20Detail.md)
- [Wi-Fi Fundamentals](./TryHackMe/WiFi%20Fundamentals.md)
- [Wi-Fi Fundamentals and Reconnaissance](./TryHackMe/Wi-Fi%20Fundamentals%20and%20Reconnaissance.md)
- [WiFi Hacking](./TryHackMe/WiFi%20Hacking.md)
- [Nmap Live Host Discovery](./TryHackMe/Nmap%20Live%20Host%20Discovery.md)
- [Operating Systems](./TryHackMe/Operating%20Systems.md)
- [Inside a Computer System](./TryHackMe/Inside%20a%20Computer%20System.md)
- [Windows Basics](./TryHackMe/Windows%20Basics.md)
- [Linux Fundamentals Part 1](./TryHackMe/Linux%20Fundamentals%20Part%201.md)
- [Python Core Concepts](./TryHackMe/Python%20Core%20Concepts.md)
- [Data Encoding](./TryHackMe/Data%20Encoding.md)
- [Cryptography for Dummies](./TryHackMe/Cryptography%20for%20Dummies.md)
- [Cryptography Basics](./TryHackMe/Introduction%20to%20Cryptography%20Basics.md)

### Offensive and web security
- [Offensive Security Intro](./TryHackMe/Offensive%20Security%20Intro.md)
- [Pentesting Fundamentals](./TryHackMe/Pentesting%20Fundamentals.md)
- [The Hacker Methodology](./TryHackMe/The%20Hacker%20Methodology.md)
- [Passive Reconnaissance](./TryHackMe/Passive%20Reconnaissance.md)
- [Active Reconnaissance](./TryHackMe/Active%20Reconnaissance.md)
- [Google Dorking](./TryHackMe/Google%20Dorking.md)
- [Basic Pentesting](./TryHackMe/Basic%20Pentesting.md)
- [Metasploit Introduction](./TryHackMe/Metasploit%20Introduction.md)
- [Hydra](./TryHackMe/Hydra.md)
- [Crack the Hash](./TryHackMe/Crack%20the%20Hash.md)
- [OverlayFS CVE-2021-3493](./TryHackMe/OverlayFS%20CVE-2021-3493.md)
- [MD2PDF](./TryHackMe/MD2PDF.md)
- [Guided Pentest: Web](./TryHackMe/Guided%20Pentest%20Web.md)
- [Guided Pentest: Infrastructure](./TryHackMe/Guided%20Pentest%20Infrastructure.md)
- [Internal](./TryHackMe/Internal.md)
- [Road](./TryHackMe/Road.md)
- [Gaming Server](./TryHackMe/Gaming%20Server.md)
- [Psycho Break](./TryHackMe/Psycho%20Break.md)
- [TryHack3M: Bricks Heist](./TryHackMe/TryHack3M%20Bricks%20Heist.md)
- [RA: Oh My WebServer](./TryHackMe/RA%20Oh%20My%20WebServer.md)

### Defensive security, forensics, and phishing
- [Defensive Security Intro](./TryHackMe/Defensive%20Security%20Intro.md)
- [Introduction to SIEM](./TryHackMe/Introduction%20to%20SIEM.md)
- [SOC Role in the Blue Team](./TryHackMe/SOC%20Role%20in%20Blue%20Team.md)
- [DFIR: An Introduction](./TryHackMe/DFIR%20An%20Introduction.md)
- [Snort](./TryHackMe/Snort.md)
- [Junior Security Analyst Intro](./TryHackMe/Junior%20Security%20Analyst%20Intro.md)
- [Phishing Basics](./TryHackMe/Phishing%20Basics.md)
- [Phishing: Merry Clickmas](./TryHackMe/Phishing%20Merry%20Clickmas.md)
- [Phishing with HiddenEye](./TryHackMe/Phishing%20HiddenEye.md)
- [Malware Introductory](./TryHackMe/Malware%20Introductory.md)
- [Malware Researching](./TryHackMe/Malware%20Researching.md)
- [Android Malware Analysis](./TryHackMe/Android%20Malware%20Analysis.md)

### AI security and newer challenges
- [The Building Blocks of AI](./TryHackMe/The%20Building%20Blocks%20of%20AI.md)
- [AI Models and Data](./TryHackMe/AI%20Models%20and%20Data.md)
- [Prompt Engineering](./TryHackMe/Prompt%20Engineering.md)
- [AI Threat Modelling](./TryHackMe/AI%20Threat%20Modelling.md)
- [AI Security Threats](./TryHackMe/AI%20Security%20Threats.md)
- [Securing AI Systems](./TryHackMe/Securing%20AI%20Systems.md)
- [LLM Security](./TryHackMe/LLM%20Security.md)
- [AI Forensics](./TryHackMe/AI%20Forensics.md)
- [AI System Reconnaissance](./TryHackMe/AI%20System%20Reconnaissance.md)
- [Cypheron](./TryHackMe/Cypheron.md)
- [Phantom Fob](./TryHackMe/Phantom%20Fob.md)
- [The Brochure](./TryHackMe/The%20Brochure.md)

### Other challenge notes
- [Search Skills](./TryHackMe/Search%20Skills.md)
- [Ninja Skills](./TryHackMe/Ninja%20Skills.md)
- [Cyber Heroes](./TryHackMe/Cyber%20Heroes.md)
- [Advent of Cyber 2022](./TryHackMe/Advent%20of%20Cyber%202022.md)
- [Advent of Cyber 2023](./TryHackMe/Advent%20of%20Cyber%202023.md)
- [Advanced SQL Injection: Advent of Cyber 2024](./TryHackMe/Advanced%20SQL%20Injection%20Advent%20of%20Cyber%202024.md)
- [Advent of Cyber Prep Track](./TryHackMe/Advent%20of%20Cyber%20Prep%20Track.md)

---

### note

i will try to make it Clear, actionable technical logging with clean syntax highlighting and code preservation.

and also feel free to ask a questions if anything is unclear.

---

> **Break it legally. Understand it. Secure it.** 🔐
