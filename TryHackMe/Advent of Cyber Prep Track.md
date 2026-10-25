# Advent of Cyber Prep Track — TryHackMe

Room: https://tryhackme.com/room/adventofcyberpreptrack

This is my walkthrough for the **10 interactive warm-up challenges** in the Advent of Cyber 2025 Prep Track. This room is not a 24-day event itself: it has ten small missions, each teaching a different basic skill. The full Advent of Cyber event is a separate collection of daily rooms.

I am writing down the actual sequence for each mini-challenge instead of only listing the final result. The answer strings are redacted as `flag{hidden}`, but the steps and reasoning are kept so I can repeat the task without needing a copied answer.

## Before starting

1. Open the room on TryHackMe and read the introduction.
2. For each mini-challenge, press **View Site** in the top-right of that task. Each task loads its own interactive site, so if the next task does not appear, click View Site again.
3. Keep the instructions on one side and the interactive challenge on the other.
4. Follow the task in its own simulation. The exercises below are not permission to test a real router, account, suspicious executable, or third-party service.

---

## Challenge 1 — Password Pandemonium

### What the task is testing

The site says that weak passwords have been detected at TBFC. My job is to create a password that passes the application's checks and is not in its simulated leaked-password list. This is a password-quality exercise, not a password-cracking task.

### Steps

1. Open **Challenge 1 — Password Pandemonium** and click **View Site**.
2. Read the password requirements shown by the application before entering anything.
3. Create a fresh practice password with at least 12 characters.
4. Make sure it contains uppercase letters, lowercase letters, numbers, and symbols. The example password mentioned in the story is already known to the checker, so copying it will not satisfy the breached-password check.
5. Enter the new password in the simulated form.
6. If the site rejects it, read the specific validation message. Change the password to address that failed requirement rather than randomly trying short variations.
7. Submit once all checks pass. The application displays the flag for this challenge.

### Why this works

Length and variety make guessing harder, but the breached-password check matters too. A password can satisfy character rules and still be a poor choice if it is common or has already appeared in a breach. In real accounts, I should use a unique password generated and stored by a password manager rather than reuse a practice password.

**Flag:** `flag{hidden}`

---

## Challenge 2 — The Suspicious Chocolate.exe

### What the task is testing

The scenario gives me a file named `chocolate.exe` and a simulated VirusTotal-style scanning page. The task is to decide whether the file is safe or malicious by reading the scan report. The name and festive story are not evidence that the executable is trustworthy.

### Steps

1. Open **Challenge 2 — The Suspicious Chocolate.exe** and click **View Site**.
2. Find the file entry for `chocolate.exe`.
3. Click the **Scan** button. Wait for the simulated report to load.
4. Read the result counts instead of judging by the filename. The room's report shows 49 clean results and 1 malicious result.
5. Inspect the detection details. The report identifies a suspicious detection associated with **Malhare Labs**, with the label **MalhareTrojan** and reference **ML-2025-011**.
6. Since at least one engine has flagged the file and the details identify a malicious detection, do not classify it as safe.
7. Select the malicious/dangerous answer in the simulated interface and submit it. The site then provides the task flag.

### Why this works

A multi-engine scan is evidence, not absolute proof. False positives can happen, and a clean result does not guarantee a file is safe. In a real investigation I would correlate the hash, source, behaviour, signatures, and sandbox observations. For this training task, however, I should answer from the report the challenge actually provides.

**Flag:** `flag{hidden}`

---

## Challenge 3 — Welcome to the AttackBox!

### What the task is testing

This challenge is a basic Linux command-line exercise. I need to locate and read a text file in the AttackBox. The important commands are `ls`, `cd`, and `cat`.

### Steps

1. Open **Challenge 3 — Welcome to the AttackBox!** and click **View Site**.
2. Start or open the AttackBox when the room asks me to. Use the terminal in the supplied training environment.
3. First, check the current directory:

   ```bash
   pwd
   ```

   This prints the current working directory, so I know where relative paths begin.
4. List the files and directories:

   ```bash
   ls
   ```

   I should see the `challenges` directory mentioned in the instructions.
5. Move into that directory:

   ```bash
   cd challenges/
   ```

6. Confirm the file is there:

   ```bash
   ls -l
   ```

7. Read the welcome message:

   ```bash
   cat welcome.txt
   ```

8. The file prints a short welcome message and the challenge flag. Submit that flag in the room's answer field.

### If the command fails

If `cd challenges/` says the directory does not exist, I should run `pwd` and `ls -la` to check where I am. If `cat welcome.txt` says the file is missing, I should list the current directory again rather than assume the command itself is wrong. Linux paths are case-sensitive.

### Why this works

`ls` lists directory contents, `cd` changes the working directory, and `cat` prints a file's contents. These are basic commands I will use constantly during investigations.

**Flag:** `flag{hidden}`

---

## Challenge 4 — The CMD Conundrum

### What the task is testing

This challenge introduces Windows Command Prompt and hidden files. A directory listing may hide files with the hidden attribute unless I ask Windows to show all attributes.

### Steps

1. Open **Challenge 4 — The CMD Conundrum** and click **View Site**.
2. Use the Windows terminal/Command Prompt supplied by the challenge.
3. List the ordinary visible entries:

   ```bat
   dir
   ```

4. Look for the `mystery_data` directory described in the scenario. If it is not visible, include hidden entries:

   ```bat
   dir /a
   ```

   The `/a` switch displays files with attributes, including hidden files.
5. Enter the directory:

   ```bat
   cd mystery_data
   ```

6. List its contents so I can confirm the filename:

   ```bat
   dir /a
   ```

7. Read the hidden flag file:

   ```bat
   type hidden_flag.txt
   ```

   If the task's current directory is different, the equivalent path shown in the room is `type mystery_data\hidden_flag.txt`.
8. Copy the returned value into the answer field in TryHackMe. In this public write-up, I replace the actual value with a placeholder.

### Why this works

`dir` lists directory contents, `dir /a` includes entries with hidden/system attributes, and `type` prints a text file in Command Prompt. This is useful when investigating a Windows host through a shell rather than its graphical file manager.

**Flag:** `flag{hidden}`

---

## Challenge 5 — Linux Lore

### What the task is testing

This challenge is similar to the AttackBox task, but it focuses on Linux hidden files. In Linux, a filename beginning with a dot is conventionally treated as hidden by ordinary directory listings.

### Steps

1. Open **Challenge 5 — Linux Lore** and click **View Site**.
2. Use the terminal provided by the challenge.
3. Move to McSkidy's home directory:

   ```bash
   cd /home/mcskidy/
   ```

4. List all entries, including hidden ones:

   ```bash
   ls -la
   ```

   The `-l` option gives a detailed listing and `-a` includes names beginning with a dot.
5. Find the hidden file named `.secret_message`.
6. Read it:

   ```bash
   cat .secret_message
   ```

7. Submit the value printed by the file to the task's answer field.

### If the file is not found

First check the path:

```bash
pwd
ls -la /home/mcskidy/
```

Make sure the username and filename match the challenge exactly. A common mistake is to misspell a directory or forget the leading dot in the filename.

### Why this works

Linux hides dotfiles from a plain `ls` listing, but the files are not encrypted or protected just because their names begin with a dot. File permissions still control access. Hidden filenames are a convenience, not a security boundary.

**Flag:** `flag{hidden}`

---

## Challenge 6 — The Leak in the List

### What the task is testing

The challenge uses a simulated breach checker to show whether McSkidy's email address appears in a breach. I need to identify the domain marked as compromised and use the simulated results to finish the task.

### Steps

1. Open **Challenge 6 — The Leak in the List** and click **View Site**.
2. Find the email lookup field.
3. Enter the exact training address:

   ```text
   mcskidy@tbfc.com
   ```

4. Submit the lookup and wait for the results.
5. Review the result for each domain shown by the simulated checker. Do not assume that every listed domain is compromised.
6. Find the entry whose status is **Compromised**.
7. Select the matching domain or status requested by the page and submit the answer.
8. The simulation displays the flag after the correct result is selected.

### Why this matters

Breach-notification services can help people discover that an account appears in known leaked data. A match does not prove that an account is currently compromised, but it is a reason to change the affected password, check for password reuse, enable MFA, and review account activity. I should never submit someone else's private email address to an external service without a legitimate reason.

**Flag:** `flag{hidden}`

---

## Challenge 7 — WiFi Woes in Wareville

### What the task is testing

The story describes a router that still uses default login credentials. The exercise asks me to log into a simulated router and change its password. The lesson is that default credentials must not remain in use.

### Steps

1. Open **Challenge 7 — WiFi Woes in Wareville** and click **View Site**.
2. Use the login form in the simulated router interface.
3. Enter the default credentials specified by the room:

   - Username: `admin`
   - Password: `admin`

4. After logging in, find **Security Settings**.
5. Locate the router password field. Do not confuse the administrator password with the Wi-Fi network's SSID; the name of the network and its authentication secret serve different purposes.
6. Create a new, unique password that satisfies the validation rules displayed by the simulator. Use a long passphrase that is not based on the default credential or a common password.
7. Enter the new value in the required field(s), save the settings, and confirm the simulator accepts the change.
8. Submit the task as directed. The page reveals the flag when the simulated router is secured correctly.

### Why this works

Default credentials are widely known and often tested automatically. Changing them prevents someone from using the manufacturer's default login. Real routers should also have updated firmware, remote administration disabled unless needed, WPA2-AES or WPA3 enabled, and guest devices isolated from trusted systems.

**Flag:** `flag{hidden}`

---

## Challenge 8 — The App Trap

### What the task is testing

A connected third-party application may be able to access data from an account. The challenge asks me to identify the suspicious app from its permissions and revoke its access.

### Steps

1. Open **Challenge 8 — The App Trap** and click **View Site**.
2. Inspect the list of applications connected to the simulated account.
3. Open the details for each suspicious-looking application and read the permissions. Do not select an app based only on its name.
4. Pay particular attention to permissions that are far broader than the app needs, such as access to a password vault or other highly sensitive information.
5. Compare the app's stated purpose with the permissions it requests. A simple app should not need unrestricted access to unrelated secrets.
6. Select the app whose permissions match the suspicious behaviour described by the task.
7. Click **Revoke Access** and confirm the action if the interface asks.
8. Submit or finish the task as instructed. The simulated site displays the flag when the malicious connection has been removed.

### Why this works

OAuth-style connections can give third-party apps continuing access to an account. Revoking a connection removes that grant, although it may not delete data the app already copied. In real accounts, I should review connected applications periodically and change credentials if a suspicious app may have accessed them.

**Flag:** `flag{hidden}`

---

## Challenge 9 — The Chatbot Confession

### What the task is testing

The challenge shows a conversation with FestiveBot, a simulated AI assistant. Some messages reveal sensitive information such as internal URLs or passwords. My task is to select the messages that disclose private data.

### Steps

1. Open **Challenge 9 — The Chatbot Confession** and click **View Site**.
2. Read the entire conversation rather than selecting messages based on tone or wording.
3. For each message, ask whether it reveals information that should not be public. Examples include:
   - Passwords, tokens, or other authentication secrets.
   - Internal-only URLs or service addresses.
   - Private company data that the chatbot should not disclose.
4. Select each message that contains sensitive information, following the page's selection method.
5. Leave ordinary, non-sensitive messages unselected.
6. Submit the selections. If the task says the answer is incorrect, review the content of each message again and check whether a secret or internal resource is exposed.
7. The challenge displays the flag after the sensitive messages have been correctly identified.

### Why this matters

An AI assistant should not automatically be trusted with secrets just because it can produce fluent answers. Applications need access controls around retrieved data, secret filtering, careful tool permissions, and testing for prompt-injection or data-disclosure paths. A system prompt alone is not a dependable security boundary.

**Flag:** `flag{hidden}`

---

## Challenge 10 — The Bunny's Browser Trail

### What the task is testing

This challenge introduces web logs and user-agent strings. A user-agent field normally identifies the client software making an HTTP request. Automated tools can use unusual user-agent strings, but the field is supplied by the client and can be forged.

### Steps

1. Open **Challenge 10 — The Bunny's Browser Trail** and click **View Site**.
2. Read the HTTP log entries shown by the challenge.
3. Identify the user-agent field in each entry. Compare the values rather than judging a request by its URL alone.
4. Common browsers often identify themselves with strings containing names such as Chrome, Firefox, or Edge. The scenario calls out this unusual value:

   ```text
   BunnyOS/1.0 (HopSecBot)
   ```

5. Find the log entry containing that user-agent string.
6. Select the suspicious entry requested by the task and submit the answer.
7. The page returns the final flag for the prep track challenge.

### Why this works

A user-agent string is a useful clue for identifying automation or an unfamiliar client, but it is not strong authentication. Any client can choose what string to send. Defenders should correlate it with request rate, paths accessed, response codes, IP reputation, and other telemetry before deciding what the traffic means.

**Flag:** `flag{hidden}`

---

## Final notes

The Prep Track has ten challenges. I should not confuse it with the full Advent of Cyber event, which contains separate daily rooms.

The main skills covered here are:

- Creating a strong password and recognising breached credentials.
- Reading a malware scan report.
- Navigating Linux and Windows command-line environments.
- Finding hidden files and reading their contents.
- Interpreting breach-check results.
- Replacing default router credentials.
- Reviewing third-party application permissions.
- Identifying sensitive information exposed by an AI chatbot.
- Investigating unusual user-agent values in web logs.

For a real investigation, I should preserve the evidence, record the exact actions and results, and avoid treating a single indicator as proof. For this public walkthrough, the actual TryHackMe flag strings are intentionally redacted as `flag{hidden}`; the route to each answer remains documented.

### Reference

- Official room: https://tryhackme.com/room/adventofcyberpreptrack
- TryHackMe describes this prep track as ten warm-up missions for Advent of Cyber 2025. The interactive page is the source of truth if its interface changes.
