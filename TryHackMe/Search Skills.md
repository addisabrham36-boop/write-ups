# Search Skills

{% embed url="https://tryhackme.com/room/searchskills" %}

Mastering the research workflow: evaluating source credibility, advanced search engine syntax, specialized security search engines (Shodan, VirusTotal), vulnerability databases, and OSINT on social platforms.

---

## 1. Information Evaluation

In security research, verifying source credibility avoids falling for fraudulent solutions or fake security tools:
- **Snake oil:** Term describing fraudulent or unproven cryptographic solutions.
- **`ss` command:** Modern Linux utility replacing `netstat`. Stands for **socket statistics**.

---

## 2. Advanced Operators

Filtering Google with precision:
- Target specific document types: `filetype:pdf cyber warfare report`
- Finding command definitions: `cat` stands for **concatenate**.
- Windows `netstat` switch displaying executable binaries: `netstat -b`.

---

## 3. Specialized Search Engines & CVEs

- **Shodan:** Identifies internet-facing systems. The top country hosting `lighttpd` web servers is the **United States**.
- **VirusTotal:** Inspecting hash `2de70ca737c1f4602517c555ddd54165432cf231ffc0e21fb2e23b9dd14e7fb4`, BitDefenderFalx classifies the file as `Android.Riskware.Agent.LHH`.
- **CVE-2024-3094:** Critical upstream supply chain backdoor affecting the **`xz`** data compression utility (`xz-utils`).

---

## 4. OSINT via Social Platforms

- **LinkedIn:** Industry-standard platform to enumerate employee technical skills, technologies used by the organization, and org hierarchies.
- **Facebook / Personal Socials:** Useful in social engineering engagements for finding answers to password security questions (e.g. hometown, primary school).

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | Cryptographic product considered bogus or fraudulent | `Snake oil` |
| **Task 2** | Command replacing netstat in Linux | `ss` |
| **Task 3** | Google search limiting to PDF files with "cyber warfare report" | `filetype:pdf cyber warfare report` |
| **Task 3** | Phrase the Linux command `ss` stands for | `socket statistics` |
| **Task 4** | Top country with lighttpd servers on Shodan | `United States` |
| **Task 4** | Detection name by BitDefenderFalx for the specified hash | `Android.Riskware.Agent.LHH` |
| **Task 5** | Utility that CVE-2024-3094 refers to | `xz` |
| **Task 6** | What does the Linux command `cat` stand for? | `concatenate` |
| **Task 6** | Windows netstat parameter displaying associated executables | `-b` |
| **Task 7** | Social media website to learn about technical background of employees | `LinkedIn` |
| **Task 7** | Social media website to check for personal answers like childhood school | `Facebook` |
