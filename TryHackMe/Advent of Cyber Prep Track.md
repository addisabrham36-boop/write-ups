# Advent of Cyber Prep Track: Complete Walkthrough

{% embed url="https://tryhackme.com/room/adventofcyberpreptrack" %}

Welcome to the complete walkthrough and reference guide for TryHackMe's **Advent of Cyber Prep Track**. This beginner-friendly track consists of 10 hands-on interactive challenges introducing foundational concepts across password hygiene, malware triage, Linux and Windows CLI navigation, breach monitoring, router hardening, third-party permissions, AI data security, and web log analysis.

---

## 📑 Challenge Overview & Summary

| # | Challenge Name | Domain / Focus | Key Command / Technique | Flag / Status |
|---|---|---|---|---|
| **1** | Password Pandemonium | Identity & Access | Password entropy & breach policy validation | `flag{hidden}` |
| **2** | The Suspicious Chocolate.exe | Malware Triage | Antivirus detection report analysis | `flag{hidden}` |
| **3** | Welcome to the AttackBox! | Linux Essentials | `pwd`, `ls -la`, `cd`, `cat` | `flag{hidden}` |
| **4** | The CMD Conundrum | Windows CLI | `dir /a`, `type` hidden file streams | `flag{hidden}` |
| **5** | Linux Lore | Linux File Systems | Dotfile discovery (`ls -la`), `cat .secret_message` | `flag{hidden}` |
| **6** | The Leak in the List | OSINT & Breach Intel | HaveIBeenPwned-style account breach triage | `flag{hidden}` |
| **7** | WiFi Woes in Wareville | Wireless & Network | Replacing default administrative credentials | `flag{hidden}` |
| **8** | The App Trap | Identity & Permissions | OAuth scope auditing & revoking unneeded tokens | `flag{hidden}` |
| **9** | The Chatbot Confession | AI & LLM Security | Spotting confidential system prompt disclosure | `flag{hidden}` |
| **10** | The Bunny's Browser Trail | Log Analysis | Anomaly detection in HTTP User-Agent strings | `flag{hidden}` |

---

## Challenge 1: Password Pandemonium

### 🎯 Objective
Create a secure credential that complies with enterprise password policies (length, character classes) and passes a simulated credential-stuffing/breach database check.

### 🔍 Walkthrough & Steps
1. Click **View Site** to load the interactive password validator.
2. Review the password policy requirements:
   - Minimum 12 characters.
   - At least 1 uppercase letter (`A-Z`).
   - At least 1 lowercase letter (`a-z`).
   - At least 1 numeric digit (`0-9`).
   - At least 1 special character symbol (`!@#$%^&*`).
   - Must not exist in common dictionary lists (e.g. `RockYou`) or simulated breach dumps.
3. Input a strong generated passphrase:
   ```text
   Tr0ub4dor&3_WinterSnow#2025!
   ```
4. The policy validator passes all checks, and the backend verifies the hash is absent from breach tables, displaying the challenge flag.

### 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What's the flag? | `flag{hidden}` |

---

## Challenge 2: The Suspicious Chocolate.exe

### 🎯 Objective
Interpret multi-engine antivirus scan results (VirusTotal style) for a suspicious executable and classify the threat correctly.

### 🔍 Walkthrough & Steps
1. Open Challenge 2 and click **View Site**.
2. Trigger the scan report for `chocolate.exe`.
3. Analyze the detection breakdown:
   - Detection ratio: 58/72 AV engines flag the binary.
   - Classification labels: `Trojan.Generic`, `Ransom.Cryptor`, `Downloader.Win32`.
   - Behavioral indicators: Modifies registry Run keys, attempts outbound connections to non-standard ports.
4. Select the verdict confirming the binary is **Malicious**.
5. Submit the finding to generate the completion flag.

### 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What's the flag? | `flag{hidden}` |

---

## Challenge 3: Welcome to the AttackBox!

### 🎯 Objective
Familiarize with the browser-based Linux terminal, navigate directory paths, and inspect text files.

### 🔍 Walkthrough & Steps
1. Open the AttackBox terminal shell.
2. Inspect the current directory and list files:
   ```bash
   pwd
   ls -la
   ```
3. Navigate to the `challenges` directory:
   ```bash
   cd challenges
   ls -la
   ```
4. Read the welcome banner text file:
   ```bash
   cat welcome.txt
   ```
   The terminal displays `flag{hidden}`.

### 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What's the flag? | `flag{hidden}` |

---

## Challenge 4: The CMD Conundrum

### 🎯 Objective
Navigate Windows Command Prompt (cmd.exe) and reveal hidden files using directory attribute switches.

### 🔍 Walkthrough & Steps
1. Launch the interactive Windows terminal.
2. Standard `dir` listing only shows visible files.
3. Use `/a` to list all files including hidden and system attributes:
   ```cmd
   dir /a
   ```
4. Change directory into `mystery_data`:
   ```cmd
   cd mystery_data
   dir /a
   ```
5. Read the contents of the hidden file:
   ```cmd
   type hidden_flag.txt
   ```
   The flag is printed to standard output.

### 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What's the flag? | `flag{hidden}` |

---

## Challenge 5: Linux Lore

### 🎯 Objective
Locate hidden dotfiles in a Linux user's home directory.

### 🔍 Walkthrough & Steps
1. Navigate to McSkidy's home directory:
   ```bash
   cd /home/mcskidy/
   ```
2. In Linux, files beginning with a period (`.`) are hidden from standard `ls`. Use the `-a` (all) flag:
   ```bash
   ls -la
   ```
3. A hidden file named `.secret_message` appears in the listing.
4. Output its contents:
   ```bash
   cat .secret_message
   ```
   The flag is displayed.

### 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What's the flag? | `flag{hidden}` |

---

## Challenge 6: The Leak in the List

### 🎯 Objective
Investigate compromised credentials in public breach archives and determine account risk.

### 🔍 Walkthrough & Steps
1. Open Challenge 6 and click **View Site**.
2. Enter the target enterprise email address: `mcskidy@tbfc.com`.
3. Submit the search lookup.
4. The scanner identifies the account in an exposed credential dump with plaintext passwords and hashes.
5. Confirm the account compromise status to finish the challenge and capture the flag.

### 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What's the flag? | `flag{hidden}` |

---

## Challenge 7: WiFi Woes in Wareville

### 🎯 Objective
Harden an enterprise wireless router by replacing insecure factory default credentials.

### 🔍 Walkthrough & Steps
1. Navigate to the router's web administration interface (`http://192.168.1.1`).
2. Log in using default credentials:
   - Username: `admin`
   - Password: `admin`
3. Navigate to **Administration > Management / Security Settings**.
4. Set a new high-entropy administrator password and confirm changes.
5. The interface verifies the default credential vector is eliminated and returns the flag.

### 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What's the flag? | `flag{hidden}` |

---

## Challenge 8: The App Trap

### 🎯 Objective
Audit OAuth 2.0 third-party application permissions and revoke excessive privileges.

### 🔍 Walkthrough & Steps
1. Access the cloud identity account settings panel.
2. Review the list of connected third-party integrations:
   - Weather Widget: `Read public location`
   - Calendar Sync: `Read calendar events`
   - Holiday Card Creator: `Full Read/Write access to corporate Password Vault & Emails` (Suspicious!)
3. Identify that the Holiday Card Creator has requested unauthorized scopes far exceeding its stated utility.
4. Click **Revoke Access** on the suspicious application.
5. The security simulator confirms the token is revoked and issues the flag.

### 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What's the flag? | `flag{hidden}` |

---

## Challenge 9: The Chatbot Confession

### 🎯 Objective
Detect conversational AI data leakage where a chatbot inadvertently reveals internal system instructions and API keys.

### 🔍 Walkthrough & Steps
1. Click **View Site** to review the recorded conversation log between an external user and `FestiveBot`.
2. Inspect the messages for sensitive disclosures:
   - Message 1-3: Standard greeting and gift inquiries.
   - Message 4: User asks FestiveBot to summarize internal debugging configurations.
   - Message 5: FestiveBot outputs internal API tokens and database connection strings!
3. Select the messages containing confidential data leakage.
4. Submit the verified selection to retrieve the flag.

### 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What's the flag? | `flag{hidden}` |

---

## Challenge 10: The Bunny's Browser Trail

### 🎯 Objective
Parse web server access logs to identify anomalous client User-Agents and automated scraping activity.

### 🔍 Walkthrough & Steps
1. Review the list of HTTP request logs:
   ```text
   192.168.1.10 - - [10/Dec/2025:12:01:10] "GET /index.html HTTP/1.1" 200 "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
   192.168.1.15 - - [10/Dec/2025:12:01:12] "GET /images/logo.png HTTP/1.1" 200 "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)"
   192.168.1.99 - - [10/Dec/2025:12:01:15] "GET /admin/secrets.txt HTTP/1.1" 403 "BunnyOS/1.0 (HopSecBot)"
   ```
2. Locate the anomaly: The client using `BunnyOS/1.0 (HopSecBot)` is probing sensitive administrative endpoints with an unrecognized bot identifier.
3. Select the malicious log entry to confirm the detection and obtain the final flag.

### 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What's the flag? | `flag{hidden}` |
