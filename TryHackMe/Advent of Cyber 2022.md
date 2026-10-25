# Advent of Cyber 2022 — full day-by-day notes

Room: https://tryhackme.com/room/adventofcyber4

This room is the **24-day Advent of Cyber 2022 event**, not the 10-challenge Prep Track. I am keeping the days separate because every day has a different target, file, tool, and objective. The exact output can depend on the lab instance, so I should use the target IP shown by TryHackMe and verify values in the running room rather than copy another person's machine address.

For this public copy, actual flags are written as `flag{hidden}`. I explain how to find them, but do not publish the secret value. Use only the lab machines launched by the room. TryHackMe's event instructions explicitly prohibit scanning or exploiting the TryHackMe infrastructure itself or other users.

## Before Day 1

1. Open the official room and read the task instructions.
2. If a task has a lab machine, click **Start Lab Machine** and wait for it to finish booting.
3. Start the AttackBox if the task needs it, or connect your own machine using the method allowed by TryHackMe.
4. Use the **View Site** button for interactive static-site tasks.
5. Keep a note of the exact question, command, important output, and what the output means. The IP address shown in the task is the only target address I should use.

---

## Day 1 — Frameworks: Someone's coming to town!

### Goal

Identify the adversary from the puzzle left after the Best Festival Company website was defaced. The learning part introduces security frameworks, especially the Unified Kill Chain, Cyber Kill Chain, and MITRE ATT&CK.

### Steps

1. Read the task's explanation of the frameworks before trying to solve the puzzle.
2. Remember the difference between the models:
   - NIST CSF groups defensive work into Identify, Protect, Detect, Respond, and Recover.
   - MITRE ATT&CK catalogues adversary tactics and techniques.
   - The Cyber Kill Chain describes broad stages of an attack.
   - The Unified Kill Chain combines attacker actions across gaining access, moving through a network, and achieving objectives.
3. Click **View Site** to open the puzzle. If the image is too small, open it in a separate browser tab and zoom in.
4. Inspect every puzzle piece and identify which Unified Kill Chain phase it represents. Do not rely on the colours alone; match the wording on the pieces to the phase descriptions in the task.
5. Use the completed sequence to identify the adversary named by the puzzle.
6. Submit the adversary name and the flag requested by the room.

### Why this works

The phases help defenders describe an intrusion as a chain of related actions instead of a random pile of alerts. Reconnaissance and delivery happen before access, while discovery, privilege escalation, and lateral movement can occur after the first foothold.

**Flag:** `flag{hidden}`

---

## Day 2 — Log Analysis: Santa's Naughty & Nice Log

### Goal

Use web-server logs to reconstruct what happened during the compromise, including the date, source IP, stolen item, and flag.

### Steps

1. Start the Day 2 lab machine. Open the AttackBox terminal if needed.
2. List the files in the starting directory:

   ```bash
   ls -lah
   ```

3. Identify the log files and locate the web-server log mentioned in the task. If there are several files, use their names and sizes to decide which one to inspect first.
4. Read a small section before searching the whole file:

   ```bash
   head -n 40 LOGFILE
   tail -n 40 LOGFILE
   ```

   Replace `LOGFILE` with the real filename shown in the lab.
5. Search for useful terms such as the stolen list's name, suspicious paths, HTTP status codes, or the attacker's user-agent. For example:

   ```bash
   grep -inE 'list|stolen|download|admin|flag|THM' LOGFILE
   ```

6. Inspect the surrounding lines for the matching event. Use timestamps to establish the date, and check the source IP field in the same request rather than assuming that the first IP in the file is the attacker.
7. Correlate related requests to identify the item accessed or stolen.
8. Search the supplied logs for the flag format used by the room and submit each answer requested by the task.

### Why this works

A log line is one event, but the sequence of lines can show a story: source, time, requested resource, and response. Filtering first makes a large log easier to investigate, while reviewing surrounding context reduces the risk of drawing the wrong conclusion.

**Flag:** `flag{hidden}`

---

## Day 3 — OSINT: Nothing escapes detective McRed

### Goal

Investigate public information and a source-code repository to discover exposed information, including credentials reused between QA and production.

### Steps

1. Read the scenario and identify the domain and repository mentioned by the task.
2. Open the provided public repository or URL. Inspect the files and commit history where the challenge directs you; do not assume the most obvious file is the only source of information.
3. Look for configuration files, environment examples, old commits, documentation, and references to QA or production services.
4. Search the repository locally if the task provides a clone or downloadable files:

   ```bash
   grep -RniE 'password|passwd|secret|token|DB_PASSWORD|prod|qa' .
   ```

5. Read the matching lines in context. A variable name alone is not the answer; identify the value actually used by the task and whether it is shown as a placeholder or a real credential.
6. Compare QA and production configuration as requested. If the same password is used in both environments, record the reused value for the room's answer field.
7. Answer the question about the exposed database password using the evidence from the supplied repository.

### Why this works

OSINT does not always mean searching social media. Public repositories and forgotten commits can reveal secrets that were removed from the latest version but remain in history. Real secrets found in a repository must be revoked and rotated, not merely deleted from the visible file.

**Flag:** `flag{hidden}`

---

## Day 4 — Scanning: Scanning through the snow

### Goal

Enumerate the authorised QA server, identify the service on port 22, access the exposed Samba service, and follow the clues to the provided credentials.

### Steps

1. Start the task's target machine and note its assigned IP.
2. Run a first TCP scan and save the output:

   ```bash
   nmap -sV -sC -oN day4-initial.txt TARGET_IP
   ```

3. Replace `TARGET_IP` with the IP from the room. `-sV` attempts to identify service versions, `-sC` runs Nmap's default scripts, and `-oN` saves readable output.
4. Inspect the open ports. For port 22, record the service name reported by Nmap rather than guessing from the port number alone.
5. If Samba/SMB is exposed, enumerate the available shares using the room's allowed tooling. A common starting point is:

   ```bash
   smbclient -L //TARGET_IP/ -N
   ```

6. If a share is accessible, connect to the share that the scan or task points to and list its contents. Download or read only the files relevant to the lab.
7. Search the supplied files for credentials or a flag, and record which share and file produced the evidence.
8. Use the discovered username and password only against the assigned lab service and answer the questions.

### Why this works

Service enumeration gives me a map of the attack surface. SMB shares sometimes expose files that reveal additional credentials, but I should verify access and document the exact evidence rather than assume every Samba service is misconfigured.

**Flag:** `flag{hidden}`

---

## Day 5 — Brute-Forcing: He knows when you're awake

### Goal

Find the password for the VNC service on the assigned machine, then connect to the desktop and read the flag shown there.

### Steps

1. Start the Day 5 machine and confirm the target IP.
2. Scan the target to find the VNC service and the port it listens on:

   ```bash
   nmap -sV -sC -oN day5-scan.txt TARGET_IP
   ```

3. Read the result and confirm which port is running VNC. Do not aim Hydra at SSH or another service by accident.
4. Use the lab's supplied username/wordlist and the VNC service with Hydra. The syntax depends on the exact service and wordlist provided by the task; follow the room's example and keep the attack scoped to the target.
5. When Hydra returns a candidate password, record it and verify it through the VNC client as instructed.
6. Open Remmina from the AttackBox's applications menu, choose the VNC protocol, and enter the target IP and VNC port.
7. Authenticate using the recovered password.
8. Read the flag displayed on the target desktop and submit it to the task.

### Why this works

The scan identifies the service, Hydra automates password guesses against the correct authentication service, and the VNC client verifies the recovered credential. A recovered password is not proof by itself until the expected service accepts it.

**Flag:** `flag{hidden}`

---

## Day 6 — Email Analysis: It's beginning to look a lot like phishing

### Goal

Inspect a suspicious email, its headers, sender reputation, and attachment metadata without opening the attachment on a normal workstation.

### Steps

1. Start the task's lab machine and open the supplied email-analysis environment.
2. Open the suspicious message and inspect its raw headers. Record the visible sender, return address, display name, and X-Spam-Score.
3. Compare the From and Return-Path fields. A mismatch can be a clue, but it is not automatically proof of phishing.
4. Inspect the Message-ID for any encoded or hidden value the task asks about. Read the exact field and decode only the portion that appears encoded.
5. Examine the attachment metadata in the lab. Record its filename and hash; do not execute it.
6. Copy the hash into the reputation-analysis services specified by the task. In VirusTotal, open the Behaviour section and read the MITRE ATT&CK tactic entries in the order requested.
7. Search the same hash using the second analysis service named in the task and record the reported file subcategory.
8. Answer each question from the corresponding field or report, not from an assumption about what a phishing email usually looks like.

### Why this works

Email headers help separate the displayed sender from the actual routing details. Hashes let me look up a file's reputation without opening it. A sandbox report is useful evidence, but its results should be interpreted in context.

**Flag:** `flag{hidden}`

---

## Day 7 — CyberChef: Maldocs roasting on an open fire

### Goal

Examine a suspicious Word document and determine which domains it attempts to contact.

### Steps

1. Start the Day 7 lab machine and locate the document supplied by the task.
2. Do not double-click the document in a normal desktop environment. Treat it as potentially malicious.
3. Identify the file type and inspect its strings or metadata using the tools available in the lab. If the room asks me to use CyberChef, open the provided offline CyberChef instance.
4. Load the data into CyberChef's input panel. Add decoding or parsing operations only when the input gives a reason to use them, such as Base64-looking text, URL encoding, or an embedded script.
5. Build the recipe one operation at a time and inspect the output after each step. This makes it easier to understand which transformation reveals the next layer.
6. Look for URLs or domains in the decoded output. Separate the domain names from surrounding script syntax and submit the domains asked for by the room.
7. If a result appears encoded more than once, repeat the appropriate decode operation on the output rather than stacking random operations.

### Why this works

CyberChef is a transformation workbench. It can decode and parse suspicious content without needing to execute the document. The goal is to expose the document's behaviour from its contents, not to run the malware to see what happens.

**Flag:** `flag{hidden}`

---

## Day 8 — Smart Contracts: Last Christmas I gave you my ETH

### Goal

Demonstrate a re-entrancy vulnerability in the provided EtherStore contract using the supplied Solidity files and Remix.

### Steps

1. Download the task files and extract the archive.
2. Open Remix in the browser and import the provided `EtherStore.sol` and `Attack.sol` files into the workspace.
3. Read both contracts before compiling. Identify the deposit and withdrawal functions in EtherStore and the fallback/receive behaviour in the attack contract.
4. Select the compiler version specified by the room and compile each contract individually. If compilation fails, check the compiler version and source pragma before changing code.
5. In **Deploy & Run Transactions**, select the local test environment specified by the task. Deploy EtherStore first.
6. Fund the EtherStore contract using the deposit function and the amount specified by the exercise. The attack needs a balance in the target contract for the vulnerable withdrawal path to be meaningful.
7. Deploy the Attack contract with the EtherStore address as its constructor argument.
8. Follow the task's call sequence: deposit into the attack contract, then invoke its attack function. The fallback/receive function can re-enter EtherStore during a withdrawal if the vulnerable contract updates its accounting after transferring funds.
9. Inspect the transaction results and contract balances. A successful demonstration shows that the attack contract can withdraw more than its legitimate deposit because the target's balance check and state update are ordered unsafely.
10. Read the flag produced by the local exercise and submit it.

### Why this works

The vulnerable pattern sends Ether before updating the caller's stored balance. During the external call, the attack contract can call back into the withdrawal function while the old balance is still recorded. The standard fix is checks-effects-interactions: validate conditions, update internal state, then make external calls, along with appropriate re-entrancy protection.

**Flag:** `flag{hidden}`

---

## Day 9 — Pivoting: Dock the halls

### Goal

Exploit the authorised web application in the lab, recognise that it is running in a Docker container, and pivot from that foothold to discover the internal host and reach the final flag.

### Steps

1. Start the target machine and run the initial scan:

   ```bash
   nmap -T4 -A -Pn -oN day9-scan.txt TARGET_IP
   ```

2. Inspect the HTTP service and open the web page. Identify the framework and version from evidence in the page, headers, or task clues.
3. Research the vulnerability referenced by the room and check that it matches the observed application version. Use only the exploit path the lab authorises.
4. If a shell session opens, verify the session and upgrade it to Meterpreter only with the command the room teaches for that session type.
5. Check whether the shell is inside a container. A file such as `/.dockerenv` is one clue that a process is running in Docker, though it should be corroborated with other evidence.
6. Inspect the application configuration files for database connection settings. Follow the path indicated by the room rather than searching unrelated files.
7. Use the discovered database credentials only in the lab. Identify the relevant database and table, then retrieve the application credentials required by the challenge.
8. Determine the internal network and host services using the compromised container as the pivot point. The lab describes SOCKS proxying and ProxyChains; configure the proxy as shown in the task and route scans through it.
9. Enumerate only the internal services in scope, identify the next service the task points to, and use the discovered credentials as intended.
10. Follow the room's final privilege-escalation path and read the root flag.

### Why this works

Pivoting uses a compromised machine as a bridge to network resources that were not directly reachable from the attacker's original position. Container boundaries can reduce exposure, but they should not be treated as an automatic guarantee of isolation.

**Flag:** `flag{hidden}`

---

## Day 10 — Hack a Game: You're a mean one, Mr. Yeti

### Goal

Use a browser game and the Cetus memory editor to find and change a value held in the running game's memory.

### Steps

1. Start the task machine and open the game page in the provided browser.
2. Open the Cetus extension/tool available in the lab and refresh the game if it is not detecting the WebAssembly application.
3. Talk to the guard in the game and try a number so the game displays the expected value.
4. In Cetus, choose the integer value type that matches the number used by the task (the room describes a 32-bit integer, `i32`).
5. Search memory for the value shown by the game. Inspect the matching addresses and bookmark the candidate that tracks the value.
6. Talk to the guard again. The random number changes, so observe which bookmarked memory value changes with it. This helps confirm the correct address.
7. Convert any hexadecimal value shown by the tool into decimal if the game expects a decimal answer.
8. Use the confirmed value to pass the guard, then continue the game and apply the task's differential-search technique to any remaining obstacles.
9. Submit the flag displayed after completing the challenge.

### Why this works

A program keeps active values in RAM while it runs. A memory editor can search for a known value and narrow down addresses as the value changes. This technique is useful for understanding program behaviour, but it should only be used on the supplied game or software I am authorised to test.

**Flag:** `flag{hidden}`

---

## Day 11 — Memory Forensics: Not all gifts are nice

### Goal

Investigate a memory image using Volatility to identify suspicious processes and evidence of compromise.

### Steps

1. Start the task machine and locate the supplied memory image.
2. Identify the image and the Volatility version available in the lab. The command structure for Volatility 3 is:

   ```bash
   python3 vol.py -f MEMORY_IMAGE windows.info
   ```

3. Replace `MEMORY_IMAGE` with the provided file path. Record the OS/build information and acquisition details requested by the room.
4. List active processes:

   ```bash
   python3 vol.py -f MEMORY_IMAGE windows.pslist
   ```

5. Cross-check with a memory scan:

   ```bash
   python3 vol.py -f MEMORY_IMAGE windows.psscan
   ```

6. Compare the process hierarchy:

   ```bash
   python3 vol.py -f MEMORY_IMAGE windows.pstree
   ```

7. Investigate the process identified by the task. Use the relevant process ID when running targeted plugins such as `windows.dlllist` or `windows.handles`.
8. Inspect network connections with `windows.netscan` if the question asks about network activity.
9. Correlate the results instead of assuming one unusual process is malicious. Submit the requested process, parent, DLL, or handle information based on the output.

### Why this works

Memory captures can preserve evidence that is not present in disk logs, including process objects, network connections, and loaded modules. `pslist` and `psscan` use different approaches, so differences can be investigative leads.

**Flag:** `flag{hidden}`

---

## Day 12 — Malware Analysis: Forensic McBlue to the REVscue!

### Goal

Analyse the suspicious document or malware artefact from the phishing investigation and determine what it does without executing it on an ordinary host.

### Steps

1. Start the lab machine and locate the sample.
2. Begin with static inspection:

   ```bash
   file SAMPLE
   sha256sum SAMPLE
   strings -n 6 SAMPLE | head -n 80
   ```

3. Replace `SAMPLE` with the file supplied by the room. Record the hash so the artefact can be identified consistently.
4. Follow the task's reverse-engineering instructions to identify the file format and extract useful strings, embedded scripts, or configuration.
5. Search the output for URLs, domains, commands, registry paths, and suspicious file names. Validate any encoded values by applying the appropriate decoding step.
6. If the room provides a sandbox or analysis tool, use that isolated environment and compare the observed behaviour with the static findings.
7. Record the indicators of compromise requested by the task and submit them.

### Why this works

Static analysis can reveal embedded indicators without executing the sample. Dynamic analysis can show what the sample actually does, but it must happen in a controlled sandbox. Combining both views is more reliable than relying on a single antivirus label.

**Flag:** `flag{hidden}`

---

## Day 13 — Packet Analysis: Simply having a wonderful pcap time

### Goal

Investigate a packet capture to identify suspicious traffic and answer questions about the network activity.

### Steps

1. Start the lab machine and locate the provided PCAP file.
2. Open it in Wireshark from the desktop, or use command-line tools if the task asks for them.
3. Start with a high-level view: protocol hierarchy, conversations, endpoints, and timestamps. This helps identify which protocols and hosts dominate the capture.
4. Filter the traffic based on the question. Examples include:

   ```text
   dns
   http
   tcp
   ip.addr == TARGET_IP
   ```

5. Follow a TCP stream when the task points to a particular HTTP or text-based exchange. Inspect the request and response together so that the meaning is clear.
6. For DNS, inspect query names and responses. For HTTP, review hosts, URIs, status codes, and transferred content. For suspicious binary data, export the relevant object only if the task requires it.
7. Use the packet timestamps and endpoint addresses to answer the questions. Do not assume that the first packet belongs to the attacker.
8. Search for the room's flag in the provided traffic or extracted object and submit it.

### Why this works

A PCAP preserves network conversations that may not be available in ordinary application logs. Filters narrow the view, while stream reconstruction can turn separate packets into readable application data.

**Flag:** `flag{hidden}`

---

## Day 14 — Web Applications: I'm dreaming of secure web apps

### Goal

Find an insecure direct object reference (IDOR) in the internal web application and access the data the challenge expects.

### Steps

1. Start the task's web application and open it in the split view.
2. Log in using the test account or credentials supplied by the room.
3. Open the profile page and inspect the URL and requests in the browser developer tools or proxy. Look for an object identifier that changes between records.
4. Note the identifier used for the logged-in test account.
5. Change only the object identifier in the authorised lab request to the adjacent or specifically referenced record described by the task.
6. Compare the response. If the application returns another user's data without checking permission, that is evidence of IDOR.
7. The room also points out that stored images can be vulnerable. Start with a valid profile-image URL and inspect how the application selects the image; test the identifier or path only as instructed by the lab.
8. Record the office number and retrieve the hidden flag requested by the task.

### Why this works

An identifier is not a permission check. The server must verify that the current user is authorised to access the requested object on every request, including image and file endpoints.

**Flag:** `flag{hidden}`

---

## Day 15 — Secure Coding: Santa is looking for a Sidekick

### Goal

Investigate an unrestricted file-upload vulnerability and identify appropriate defensive controls.

### Steps

1. Start the task machine and open the web application.
2. Inspect the upload form and the types of files it claims to accept.
3. Review the supplied application code or behaviour to determine which checks are performed on uploaded files.
4. Follow the task's controlled upload test to determine whether the application accepts a file type it should reject. Do not upload malware to any system outside the lab.
5. Check how the application stores uploaded files. If a predictable original filename is used directly, record that as a separate weakness.
6. Follow the task's file-access path to locate the HR Elf's Documents directory and the flag the challenge asks for.
7. For the mitigation questions, distinguish the controls:
   - Allowlist permitted file types and verify the actual content, not only the extension.
   - Generate random server-side filenames so a user cannot guess a stored path from the original name.
   - Scan or sandbox files where appropriate, and store uploads outside executable web directories.
8. Submit the answers requested by the room.

### Why this works

A secure upload process needs layered controls. File extensions and browser-side checks are not sufficient on their own. The server should validate the file, store it safely, restrict execution, and enforce access controls on retrieval.

**Flag:** `flag{hidden}`

---

## Day 16 — Secure Coding: SQLi's the king, the carolers sing

### Goal

Understand SQL injection in the supplied application and identify the values exposed by the vulnerable query.

### Steps

1. Start the lab machine and open the application or code supplied by the room.
2. Identify the input that is used in a database query. Read the surrounding code and determine whether user input is concatenated directly into SQL.
3. Use the task's test input in the lab and compare the response with an ordinary valid input. Do not try payloads against an unrelated service.
4. Follow the task's query logic to determine why the input changes the meaning of the SQL statement.
5. Work through the room's successive questions in order. Each flag or value should come from the response or data source identified by that step, not from guessing.
6. If the room asks about a fix, use parameterised queries and least-privilege database credentials as the main recommendations.

### Why this works

SQL injection occurs when data is interpreted as query syntax. Parameterised queries separate those two roles, preventing user-controlled values from altering the structure of the query.

**Flags:** `flag{hidden}` for each actual flag in the room.

---

## Day 17 — Secure Coding: Filtering for Order Amidst Chaos

### Goal

Learn to validate input and use filtering to accept only data that matches the application's expected format.

### Steps

1. Start the task site and read the explanation of input validation.
2. Identify the expected structure of the values being checked. The task includes URL examples and asks how many match a specified syntax and how many start with HTTPS.
3. Read the exact regular expression or syntax rule shown by the room before counting anything.
4. Evaluate each supplied URL against the full rule. Check the scheme, separators, hostname, and any required path or characters.
5. Count the values that satisfy the entire pattern. Do not count a URL simply because it contains the expected text somewhere in the middle.
6. From the valid set, count those whose scheme begins with `https` as required by the question.
7. Submit both counts and any other values requested by the room.

### Why this works

Input validation should match the application's real requirements. Syntax validation checks whether the value is shaped correctly; semantic validation checks whether it makes sense in context. Allowlisting is usually safer than trying to block a few known-bad strings.

**Flag:** Only include a flag if the task actually provides one; otherwise, there is no flag section for this day.

---

## Day 18 — Sigma: Lumberjack Lenny Learns New Rules

### Goal

Use Sigma detection rules to identify suspicious events in supplied logs and extract the evidence requested by the three challenges.

### Steps

1. Start the lab machine and locate the three challenge logs or datasets.
2. Read the task's introduction to Sigma. A Sigma rule is a platform-independent description of a detection that can later be converted to a query for a specific SIEM.
3. Open the first challenge log and identify the event the task describes. Look for the rule's relevant fields, such as event ID, user account, process, or command line.
4. Match the event to the corresponding Sigma detection logic and answer the first challenge's questions, including the account that was created.
5. Repeat for Challenge 2. Inspect the relevant log entry and record the user's path exactly as represented in the file.
6. Repeat for Challenge 3. Calculate or read the requested MD5 hash using the exact file or value specified by the task; do not substitute SHA-256 or hash a different artefact.
7. Record each challenge flag as `flag{hidden}` in this public copy, and answer the related evidence questions.

### Why this works

Sigma makes detection logic easier to share across different security platforms. The log evidence still matters: a rule is useful only when it identifies the behaviour it was designed to detect and does not generate excessive false positives.

**Flags:** `flag{hidden}` for each actual flag in the room.

---

## Day 19 — Hardware Hacking: Wiggles go brrr

### Goal

Learn how a logic analyser can inspect communication between hardware components and understand the differences between USART, SPI, and I²C.

### Steps

1. Start the task machine and read the hardware communication introduction.
2. Learn the role of the logic analyser: it observes digital signals on wires so the investigator can inspect timing and decoded data.
3. Compare the three protocols:
   - USART uses transmit/receive signals and configuration such as baud rate.
   - SPI typically uses clock, controller-out/controller-in data lines, and one or more chip-select lines.
   - I²C uses two shared lines, SDA and SCL, with device addresses.
4. Inspect the task's captures and determine which protocol is being used from the signal pattern and decoder output.
5. Follow the task's serial-console instructions to observe the negotiation between the microprocessor and ESP32.
6. Record the new baud rate shown by the capture and read the transmitted flag after the new rate is accepted.
7. Answer the protocol comparison questions based on the task's descriptions and the observed signals.

### Why this works

A logic analyser turns electrical transitions into a timeline that can be decoded into protocol messages. The number of wires and the supported speeds depend on the protocol mode and hardware, so I should use the task's stated configuration rather than a universal rule.

**Flag:** `flag{hidden}`

---

## Day 20 — Firmware: Binwalkin' around the Christmas tree

### Goal

Inspect firmware artefacts, reverse the provided encrypted firmware, identify the passphrase for the unsigned binary, and find the root filesystem build number.

### Steps

1. Start the lab machine and locate the supplied firmware files.
2. Identify each file type before opening it:

   ```bash
   file firmwarev2.2-encrypted.gpg firmwarev1.0_unsigned
   sha256sum firmwarev2.2-encrypted.gpg firmwarev1.0_unsigned
   ```

3. Follow the task's instructions to decrypt the encrypted firmware using the provided key or passphrase mechanism. Do not invent a passphrase; derive it from the earlier evidence in the room.
4. After decrypting, inspect the resulting firmware image with the tools provided. A typical first-pass command is:

   ```bash
   binwalk firmware-image
   ```

5. Look for filesystem signatures and embedded components. If the room asks for extraction, use the extraction option available in the lab's Binwalk version and inspect the extracted directory.
6. Search the extracted files for build metadata and the root filesystem version. Configuration files, manifests, and version strings may contain the requested build number.
7. Use the exact file and task prompt to identify the passphrase for `firmwarev1.0_unsigned`.
8. Submit the flag and build information requested by the room.

### Why this works

Firmware images often contain compressed filesystems, bootloaders, configuration files, and application binaries. Binwalk identifies embedded formats so an analyst can inspect them without booting the firmware on real hardware.

**Flag:** `flag{hidden}`

---

## Day 21 — MQTT: Have yourself a merry little webcam

### Goal

Enumerate the MQTT service, inspect its version and topics, and view the camera stream in the controlled lab.

### Steps

1. Start the lab machine and record its IP address.
2. Scan the target's ports and services:

   ```bash
   nmap -sV -sC -p- -oN day21-scan.txt TARGET_IP
   ```

3. Read the output for the MQTT broker, usually Mosquitto, and record the actual port and version reported by the lab.
4. Check whether the default Nmap script output includes the `device/init` topic. The question is asking about the scan result, so I should answer from the actual output rather than assume all MQTT topics are automatically enumerated.
5. Follow the room's instructions to inspect the MQTT topics and messages using the provided client tools.
6. Identify the RTSP stream URL from the lab evidence and open it using the tool or media player specified by the task.
7. Inspect the video feed and record the flag shown in the authorised stream.

### Why this works

MQTT is a publish/subscribe messaging protocol often used by IoT devices. Poorly secured brokers can expose topics and messages. RTSP can carry a live media stream, so access control and network segmentation matter for camera devices.

**Flag:** `flag{hidden}`

---

## Day 22 — Attack Surface Reduction: Threats are failing all around me

### Goal

Complete the interactive static-site exercise to reduce the attack surface and choose appropriate controls for the scenario.

### Steps

1. Click **View Site** to open the task's static exercise.
2. Read the scenario and list the systems, services, accounts, or features that are exposed.
3. For each proposed change, identify what it removes or restricts: unused services, unnecessary privileges, exposed management interfaces, weak authentication, or unnecessary public access.
4. Apply the task's choices in the provided site. Prefer the option that reduces unnecessary exposure while preserving the required business function.
5. Review the result before submitting. If the page indicates that the environment is still exposed, revisit the remaining public services or overly broad permissions.
6. Submit the final answer or flag shown by the exercise.

### Why this works

Attack-surface reduction removes opportunities that an attacker could otherwise probe. It is not only about adding a firewall: disabling unused features, limiting access, removing default accounts, and reducing privileges all lower risk.

**Flag:** `flag{hidden}`

---

## Day 23 — Defence in Depth: Mission ELFPossible, Abominable for a Day

### Goal

Complete the interactive cases by combining multiple clues to recover vault passwords and flags. Each case adds more evidence, so I should solve them in order rather than jump straight to the final case.

### Steps

1. Click **View Site** and open Case 1.
2. Read all the clues in Case 1 and determine the password for Santa's Vault using only the information provided. Submit it and record the flag returned by the simulator.
3. Open Case 2. Read the new clue about Santa's favourite thing and use it to work out the vault password. Submit the password and record the flag.
4. Open Case 3 and read the full scenario before making guesses. This case asks for the executive assistant's favourite thing, Santa's previous and current passwords, two parts of the vault password, the combined vault password, and the flag.
5. Keep each recovered value separate in your notes. If the challenge provides password-history clues, use the previous/current distinction carefully; do not assume that an old password is still valid.
6. Combine the two password parts in the order specified by the task and test the resulting vault password in the simulator.
7. Submit the requested values for Case 3, then find Santa's Code and the final Abominable for a Day flag.

### Why this works

Defence in depth uses multiple controls so that one failure does not automatically expose the protected asset. The exercise's cases illustrate how separate pieces of information can combine into an access path. In real systems, security questions and predictable personal details are weak authentication factors and should not protect important secrets.

**Flags:** `flag{hidden}` for each actual flag in the room.

---

## Day 24 — Feedback: Ho, ho, ho, the survey's short

### Goal

Finish the event and retrieve the completion flag before submitting the feedback survey.

### Steps

1. Open the final feedback task.
2. Read the instruction carefully: the task warns me to capture the flag before clicking **Submit** on the survey.
3. Open the survey and complete the required fields.
4. Before submitting, look through the page for the completion flag and copy it into the TryHackMe answer field.
5. Submit the survey only after I have recorded the flag.
6. Answer the remaining completion question as directed by the room.

### Why this works

This is a completion task rather than a technical exploitation challenge. The important detail is the order of operations: record the flag before submitting the survey, because the page may change or no longer show the same content afterward.

**Flag:** `flag{hidden}`

---

## Final review

The event covers several different areas of security: frameworks, log analysis, OSINT, service enumeration, password testing, phishing analysis, document forensics, smart-contract vulnerabilities, pivoting, game-memory inspection, memory forensics, malware analysis, packet analysis, IDOR, secure coding, detection rules, hardware protocols, firmware analysis, MQTT/IoT, attack-surface reduction, and defence in depth.

I should use these notes as a map, but the active room remains the source of truth for target-specific values. I should not paste a target IP from a different session, claim a command succeeded when it did not, or publish a real flag. Where a question depends on live output, I need to run the step and record the actual result.

Official reference: https://tryhackme.com/room/adventofcyber4


---

# Question-by-question answer index

The day-by-day sections above explain the method. This index maps the room's questions to the evidence and expected answer. Flags are intentionally redacted for the public repository; retrieve them from the active room and submit them there.

## Day 1 — Frameworks

- **“Who is the adversary that attacked Santa's network this year?”** Answer: **The Bandit Yeti**. The room introduces the fictional attacker in the storyline.
- **“What's the flag that they left behind?”** Answer: `THM{hidden}`. Read it from the Day 1 exercise after completing the task.

## Day 2 — Log Analysis

- **“Use the ls command to list the files present in the current directory. How many log files are present?”** Answer: **2**. List the provided files in the lab directory.
- **“Elf McSkidy managed to capture the logs generated by the web server. What is the name of this log file?”** Answer: `webserver.log`.
- **“On what day was Santa's naughty and nice list stolen?”** Answer: **Friday**. Search the log timeline for the relevant request.
- **“What is the IP address of the attacker?”** Answer: `10.10.249.191`.
- **“What is the name of the important list that the attacker stole from Santa?”** Answer: `santaslist.txt`.
- **“Look through the log files for the flag. The format of the flag is: THM{}”** Answer: `THM{hidden}`. Search the supplied logs for the flag pattern.

## Day 3 — OSINT

- **“What is the name of the Registrar for the domain santagift.shop?”** Answer: **NAMECHEAP INC**. Use a WHOIS lookup and read the registrar field.
- **“Find the website's source code (repository) on github.com and open the file containing sensitive credentials. Can you find the flag?”** Answer: `THM{hidden}`. Follow the domain to its public source repository, inspect the configuration file identified by the task, and redact the flag in public notes.

## Day 4 — Scanning

- **“What is the name of the file containing passwords?”** Answer: `config.php`.
- **“What is the name of the QA server associated with the website?”** Answer: `qa.santagift.shop`.
- **“What is the DB_PASSWORD that is being reused between the QA and PROD environments?”** Answer: `S@nta2022`. This is a lab credential; never reuse it outside the room.
- **“What is the name of the HTTP server running on the remote host?”** Answer: **Apache**.
- **“What is the name of the service running on port 22 on the QA server?”** Answer: **ssh**.
- **“What flag can you find after successfully accessing the Samba service?”** Answer: `THM{hidden}`. Enumerate the authorised SMB service, access the permitted share, and read the flag file.
- **“What is the password for the username santahr?”** Answer: `santa25`. This is a lab-only credential.

## Day 5 — Brute-Forcing

- **“Use Hydra to find the VNC password of the target with IP address MACHINE_IP. What is the password?”** Answer: `1q2w3e4r`. Run the room's Hydra task against the specified VNC service and verify the candidate in the lab.
- **“Using a VNC client on the AttackBox, connect to the target of IP address MACHINE_IP. What is the flag written on the target's screen?”** Answer: `THM{hidden}`. Connect to the authorised VNC target and read the displayed flag.

## Day 6 — Email Analysis

- **“What is the email address of the sender?”** Answer: `chief.elf@santaclaus.thm`.
- **“What is the return address?”** Answer: `murphy.evident@bandityeti.thm`.
- **“On whose behalf was the email sent?”** Answer: **Chief Elf**.
- **“What is the X-spam score?”** Answer: **3**.
- **“What is hidden in the value of the Message-ID field?”** Answer: `AoC2022_Email_Analysis`.
- **“What is the reputation result of the sender's email address?”** Answer: **RISKY**.
- **“What is the filename of the attachment?”** Answer: `Division_of_labour-Load_share_plan.doc`.
- **“What is the hash value of the attachment?”** Answer: `0827bb9a2e7c0628b82256759f0f888ca1abd6a2d903acdb8e44aca6a1a03467`. Verify the hash from the supplied file if the room's artifact is available.

## Day 7 — CyberChef and Malware Triage

- **“What is the second tactic marked in the Mitre ATT&CK section?”** Answer: **Defense Evasion**.
- **“What is the subcategory of the file?”** Answer: `macro_hunter`.
- **“What is the version of CyberChef found in the attached VM?”** Answer: `9.49.0`.
- **“How many recipes were used to extract URLs from the malicious doc?”** Answer: **10**.
- **“We found a URL that was downloading a suspicious file; what is the name of that malware?”** Answer: `mysterygift.exe`.
- **“What is the last defanged URL of the bandityeti domain found in the last step?”** Answer: `hxxps[://]cdn[.]bandityeti[.]THM/files/index/`.
- **“What is the ticket found in one of the domains? (Format: Domain/<GOLDEN_FLAG>)”** Answer: `THM_MYSTERY_FLAG`.

## Day 8 — Smart Contracts

- **“What flag can you find after attacking the provided EtherStore Contract?”** Answer: `THM{hidden}`. Follow the room's controlled smart-contract exercise, verify the state change in the local blockchain, and redact the flag.

## Day 9 — Pivoting

- **“Deploy the attached VM, and wait a few minutes. What ports are open?”** Answer: **80**.
- **“What framework is the web application developed with?”** Answer: **Laravel**.
- **“What CVE is the application vulnerable to?”** Answer: `CVE-2021-3129`.
- **“What command can be used to upgrade the last opened session to a Meterpreter session?”** Answer: `sessions -u -1`.
- **“What file indicates a session has been opened within a Docker container?”** Answer: `/.dockerenv`.
- **“What file often contains useful credentials for web applications?”** Answer: `.env`.
- **“What database table contains useful credentials?”** Answer: `users`.
- **“What is Santa's password?”** Answer: `p4$$w0rd`. Treat it as a lab-only credential.

## Day 10 — Hack a Game

- **“What ports are open on the host machine?”** Answer: **22,80**.
- **“What is the root flag?”** Answer: `THM{hidden}`.
- **“What is the Guard's flag?”** Answer: `THM{hidden}`.
- **“What is the Yeti's flag?”** Answer: `THM{hidden}`.

Use the game's intended memory-editing or challenge workflow. Record the evidence shown by the simulation and do not publish flag values.

## Day 11 — Memory Forensics

- **“What is the Windows version number that the memory image captured?”** Answer: **10**.
- **“What is the name of the binary/gift that secret Santa left?”** Answer: `mysterygift.exe`.
- **“What is the Process ID (PID) of this binary?”** Answer: **2040**.
- **“Dump the contents of this binary. How many files are dumped?”** Answer: **16**.

Use the room's memory-forensics tool to identify the operating-system version, locate the process, and dump its files. The process ID and dump count are values from the supplied memory image.

## Day 12 — Malware Analysis

- **“What is the architecture of the malware sample? (32-bit/64-bit)”** Answer: **64-bit**.
- **“What is the packer used in the malware sample? (format: lowercase)”** Answer: `upx`.
- **“What is the compiler used to build the malware sample? (format: lowercase)”** Answer: `nim`.
- **“How many MITRE ATT&CK techniques have been discovered attributed to the DISCOVERY tactic?”** Answer: **2**.
- **“What is the registry key abused by the malware?”** Answer: `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`.
- **“What is the value written on the registry key based on the previous question?”** Answer: `C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\wishes.bat`.
- **“What are the names of two files created by the malware under the C:\Users\Administrator\ directory? (format: file1,file2 in alphabetical order)”** Answer: `test.jpg,wishes.bat`.
- **“What are the two domains wherein malware has initiated a network connection? (format: domain1,domain2 in alphabetical order)”** Answer: `bestfestivalcompany.thm,virustotal.com`.
- **“Going back to strings inside the malware sample, what is the complete URL used to download the file hosted in the first domain accessed by the malware?”** Answer: `http://bestfestivalcompany.thm/favicon.ico`.

Analyse the sample in the isolated environment and correlate static strings with the runtime and sandbox evidence.

## Day 13 — Packet Analysis

- **“What is the ‘Percent Packets’ value of the ‘Hypertext Transfer Protocol’?”** Answer: **0.3**.
- **“Which port number has received more than 1000 packets?”** Answer: **3389**.
- **“What is the service name of the used protocol that received more than 1000 packets?”** Answer: **RDP**.
- **“What are the domain names?”** Answer: `bestfestivalcompany[.]thm,cdn[.]bandityeti[.]thm`.
- **“What are the names of the requested files?”** Answer: `favicon[.]ico,mysterygift[.]exe`.
- **“Which IP address downloaded the executable file?”** Answer: `10[.]10[.]29[.]186`.
- **“Which domain address hosts the malicious file?”** Answer: `cdn[.]bandityeti[.]thm`.
- **“What is the ‘user-agent’ value used to download the non-executable file?”** Answer: `Nim httpclient/1.6.8`.
- **“What is the sha256 hash value of the executable file?”** Answer: `0ce160a54d10f8e81448d0360af5c2948ff6a4dbb493fe4be756fc3e2c3f900f`.
- **“What are the connected IP addresses in the mentioned pattern?”** Answer: `20[.]99[.]133[.]109,20[.]99[.]184[.]37,23[.]216[.]147[.]64,23[.]216[.]147[.]76`.

Use Wireshark display filters and packet details to correlate DNS, HTTP requests, user-agent strings, downloaded files, and hashes.

## Day 14 — Web Applications

- **“What is the office number of Elf Pivot McRed?”** Answer: **134**.
- **“Not only profile pages but also stored images are vulnerable. Start with a URL of a valid profile image; what is the hidden flag?”** Answer: `THM{hidden}`.

The task demonstrates that access-control checks must protect both profile records and associated files. Test only the lab's provided accounts and image paths.

## Day 15 — Secure Coding: File Uploads

- **“What is the name given to file uploads that allow threat actors to upload any files that they want?”** Answer: **Unrestricted**.
- **“What is the title of the web application developed by Santa's freelancer?”** Answer: `SantaSideKick2`.
- **“What is the value of the flag stored in the HR Elf's Documents directory?”** Answer: `THM{hidden}`.
- **“What defence technique can be implemented to ensure that specific file types can be uploaded?”** Answer: **File Extension Validation**.
- **“What defence technique can be used to make sure the threat actor cannot recover their file again by simply using the file name?”** Answer: **File Renaming**.
- **“What defence technique can be used to make sure malicious files that can hurt elves are not uploaded?”** Answer: **Malware Scanning**.

Use the room's application to understand the upload flaw, then explain why an extension allowlist, safe storage, file renaming, and malware scanning reduce risk. No real server should execute uploaded user files.

## Day 16 — SQL Injection

- **“What is the value of Flag1?”** Answer: `THM{hidden}`.
- **“What is the value of Flag2?”** Answer: `THM{hidden}`.
- **“What is the value of Flag3?”** Answer: `THM{hidden}`.
- **“What is the value of Flag4?”** Answer: `THM{hidden}`.

Follow the four SQL-injection exercises in the room and verify each flag from the live database response. The flags are redacted here; use the task-specific explanation in the day section above to understand the vulnerable query and the corresponding fix.

## Day 17 — Filtering for Order Amidst Chaos

- **“Filtering for Usernames: How many usernames fit the syntax above?”** Answer: **8**.
- **“Filtering for Usernames: One username consists of a readable word concatenated with a number. What is it?”** Answer: `User35`.
- **“Filtering for Emails: How many emails fit the syntax above?”** Answer: **11**.
- **“Filtering for Emails: How many unique domains are there?”** Answer: **8**.
- **“Filtering for Emails: What is the domain of the email with the local-part ‘lewisham44’?”** Answer: `amg.com`.
- **“Filtering for Emails: What is the domain of the email with the local-part ‘maxximax’?”** Answer: `fedfull.com`.
- **“Filtering for Emails: What is the local-part of the email with the domain name ‘hotmail.com’?”** Answer: `hussain.volt`.
- **“Filtering for URLs: How many URLs fit the syntax provided?”** Answer: **16**.
- **“Filtering for URLs: How many of these URLs start with ‘https’?”** Answer: **7**.

Use the supplied regex patterns and dataset. Count matches from the task file rather than manually counting a partial screen of results.

## Day 18 — Sigma Rules

- **“What is the Challenge #1 flag?”** Answer: `THM{hidden}`.
- **“From the Challenge 1 log, what user account was created?”** Answer: `BanditYetiMini`.
- **“What is the Challenge #2 flag?”** Answer: `THM{hidden}`.
- **“What was the User's path in the Challenge #2 log file?”** Answer: `SIGMA_AOC2022\Bandit Yeti`.
- **“What is the Challenge #3 flag?”** Answer: `THM{hidden}`.
- **“What was the MD5 hash associated with Challenge #3 logs?”** Answer: `2F6CE97FAF2D5EEA919E4393BDD416A7`.

A Sigma rule describes a detection in a platform-neutral format. Match each rule to the relevant event fields and confirm the alert against the supplied log.

## Day 19 — Hardware Hacking

- **“What device can be used to probe the signals being sent on electrical wires between two devices?”** Answer: **Logic Analyser**.
- **“USART is faster than SPI for communication? (Yea,Nay)”** Answer: **Nay**.
- **“USART communication uses fewer wires than SPI? (Yea,Nay)”** Answer: **Yea**.
- **“USART is faster than I2C for communication? (Yea,Nay)”** Answer: **Nay**.
- **“I2C uses more wires than SPI for communication? (Yea,Nay)”** Answer: **Nay**.
- **“SPI is faster than I2C for communication? (Yea,Nay)”** Answer: **Yea**.
- **“What is the maximum number of devices that can be connected on a single pair of I2C lines?”** Answer: **1008**.
- **“What is the new baud rate that is negotiated between the microprocessor and ESP32 chip?”** Answer: **9600**.
- **“What is the flag that is transmitted once the new baud rate was accepted?”** Answer: `THM{hidden}`.

Use the room's logic-analyser or serial-protocol simulation. Interpret the waveform and protocol fields rather than guessing from the device name.

## Day 20 — Firmware

- **“What is the flag value after reversing the file firmwarev2.2-encrypted.gpg?”** Answer: `THM{hidden}`.
- **“What is the Paraphrase value for the binary firmwarev1.0_unsigned?”** Answer: `Santa@2022`.
- **“After reversing the encrypted firmware, can you find the build number for rootfs?”** Answer: `2.6.31`.

Use the room's firmware files and tools. Preserve original files, inspect their formats, and avoid running extracted binaries on your main machine.

## Day 21 — MQTT

- **“What port is Mosquitto running on?”** Answer: **1883**.
- **“Is the device/init topic enumerated by Nmap during a script scan of all ports? (y/n)”** Answer: **y**.
- **“What Mosquitto version is the device using?”** Answer: `1.6.9`.
- **“What flag is obtained from viewing the RTSP stream?”** Answer: `THM{hidden}`.

MQTT is a publish/subscribe messaging protocol, while RTSP is used to control media streaming. Use the lab's discovery and stream-viewing steps to inspect the intended device.

## Day 22 — Attack Surface Reduction

- **“Follow the instructions in the attached static site to help McSkidy reduce her attack surface against attacks from the Yeti. Use the flag as an answer to complete the task.”** Answer: `THM{hidden}`.

The task is a guided hardening exercise. Follow the static site's instructions, apply the requested reduction steps, and copy the flag shown when the exercise confirms completion.

## Day 23 — Defence in Depth

- **“Case 1: What is the password for Santa’s Vault?”** Answer: `S3cr3tV@ultPW`.
- **“Case 1: What is the Flag?”** Answer: `THM{hidden}`.
- **“Case 2: What is Santa’s favourite thing?”** Answer: `MilkAndCookies`.
- **“Case 2: What is the password for Santa’s Vault?”** Answer: `3XtrR@_S3cr3tV@ultPW`.
- **“Case 2: What is the Flag?”** Answer: `THM{hidden}`.
- **“Case 3: What is the Executive Assistant’s favourite thing?”** Answer: `BanoffeePie`.
- **“Case 3: What is Santa’s previous password?”** Answer: `H0tCh0coL@t3_01`.
- **“Case 3: What is Santa’s current password?”** Answer: `H0tCh0coL@t3_02`.
- **“Case 3: What is the 1st part of the vault’s password?”** Answer: `N3w4nd1m`.
- **“Case 3: What is the 2nd part of the vault’s password?”** Answer: `Pr0v3dV@ultPW`.
- **“Case 3: What is the password for Santa’s Vault?”** Answer: `N3w4nd1mPr0v3dV@ultPW`.
- **“Case 3: What is the Flag?”** Answer: `THM{hidden}`.
- **“What is Santa's Code?”** Answer: `2845`.
- **“Mission ELFPossible: What is the Abominable for a Day Flag?”** Answer: `THM{hidden}`.

The exercises show how layered controls and verification can prevent one leaked password from automatically compromising every protected resource.

## Day 24 — Feedback

- **“Please help us improve by answering this 5-minute survey. Make sure to grab the flag before you click ‘Submit’!”** Answer: `THM{hidden}`. Copy the flag shown by the room before submitting the survey.
- **“Are you ready to continue your learning journey on TryHackMe? (Yea,Yea)”** Answer: **Yea**.

## Final verification notes

The values above are the expected answers from the room's task set. For dynamic infrastructure questions, use the current lab instance and its output if it differs. Never publish the actual flag values; keep them redacted as `THM{hidden}` and explain how the learner can obtain them.
