# Advent of Cyber Prep Track 2025: Complete Walkthrough

Official room: https://tryhackme.com/room/adventofcyberpreptrack

## Room Overview & Information

**Platform:** TryHackMe  
**Difficulty:** Beginner  
**Category:** Security fundamentals, malware triage, Linux and Windows CLI, credential hygiene, wireless security, OAuth-style permissions, AI data disclosure, and web log analysis.

The Prep Track contains ten short interactive challenges. Each challenge opens through **View Site**, and each interactive page is separate. Reopen View Site when moving to another task. These are simulated exercises; do not use the techniques against real accounts, networks, files, or applications.

## Challenge 1: Password Pandemonium

**Question:** “What's the flag?”

### Objective

Create a new password that satisfies the application's requirements and is not present in its simulated breach list.

### Steps

1. Open the Challenge 1 task and click **View Site**.
2. The task describes weak passwords on TBFC accounts. The example password `P@ssw0rd123` is already flagged, so do not reuse it.
3. Enter a unique practice password at least 12 characters long.
4. Include uppercase and lowercase letters, numbers, and symbols.
5. Submit it and read the validation feedback. If it fails, correct the failed requirement rather than randomly trying short variations.
6. Once all checks pass, the site displays the flag.

### Why it works

Length, uniqueness, and resistance to known-password lists are more important than merely changing one character in a common password. In real use, generate unique passwords with a password manager and never reuse a CTF password for personal accounts.

**Answer:** `THM{StrongStart}`

## Challenge 2: The Suspicious Chocolate.exe

**Question:** “What's the flag?”

### Objective

Use the simulated malware-scanning page to determine whether `chocolate.exe` should be trusted.

### Steps

1. Open Challenge 2 and click **View Site**.
2. Select **Scan** for `chocolate.exe`.
3. Wait for the scan report and inspect the result counts.
4. The report shows **49 clean results and 1 malicious result**.
5. Read the detection details. The malicious result is associated with **Malhare Labs**, detection name **MalhareTrojan**, reference **ML-2025-011**.
6. Select the answer that classifies the file as **malicious/dangerous**, then submit.

### Why it works

The room is testing basic security triage. One detection among many clean results is not automatically conclusive in a real investigation, because false positives exist, but the simulated task explicitly provides a malicious detection and expects that classification. Real analysts should also verify the file hash, source, behaviour, and sandbox results.

**Answer:** `THM{NotSoSweet}`

## Challenge 3: Welcome to the AttackBox!

**Question:** “What's the flag?”

### Objective

Use basic Linux commands to locate and read a text file.

### Steps

1. Open Challenge 3 and click **View Site**.
2. Open the provided AttackBox terminal.
3. List the current directory:

   ```bash
   ls
   ```

   `ls` lists entries in the working directory.
4. Move to the challenge directory:

   ```bash
   cd challenges/
   ```

   `cd` changes the working directory. The trailing slash is optional.
5. Confirm the file is present:

   ```bash
   ls -la
   ```

   `-l` provides a detailed listing; `-a` also includes hidden entries.
6. Read the welcome file:

   ```bash
   cat welcome.txt
   ```

7. Submit the flag printed by the file.

### Troubleshooting

If the directory is not found, run `pwd` and `ls -la` to see where the terminal started. Linux paths are case-sensitive; `welcome.txt` and `Welcome.txt` are different names.

**Answer:** `THM{Ready2Hack}`

## Challenge 4: The CMD Conundrum

**Question:** “What's the flag?”

### Objective

Use Windows Command Prompt to find and read a hidden file.

### Steps

1. Open Challenge 4 and click **View Site**.
2. Use the provided Windows command-line environment.
3. List ordinary entries:

   ```bat
   dir
   ```

4. Include files with hidden/system attributes:

   ```bat
   dir /a
   ```

   The `/a` switch lists entries regardless of their attributes, including hidden files.
5. Enter the directory:

   ```bat
   cd mystery_data
   ```

6. List it again to confirm the target filename:

   ```bat
   dir /a
   ```

7. Read the flag file:

   ```bat
   type hidden_flag.txt
   ```

   If the working directory is still the parent directory, use the fully qualified relative path:

   ```bat
   type mystery_data\hidden_flag.txt
   ```

8. Copy the exact flag shown by the challenge.

### Why it works

`dir` lists directory contents, `dir /a` includes hidden entries, and `type` writes a text file to the terminal. The hidden attribute changes how the file is displayed; it does not encrypt the contents.

**Answer:** `THM{WhereIsMcSkidy}`

## Challenge 5: Linux Lore

**Question:** “What's the flag?”

### Objective

Locate McSkidy's hidden message in a Linux home directory.

### Steps

1. Open Challenge 5 and click **View Site**.
2. In the provided Linux terminal, change to McSkidy's home directory:

   ```bash
   cd /home/mcskidy/
   ```

3. List all entries with details:

   ```bash
   ls -la
   ```

   A normal `ls` omits dotfiles by default. The `-a` option reveals filenames beginning with a dot, while `-l` displays details such as permissions, owner, group, size, and timestamp.
4. Read the hidden message:

   ```bash
   cat .secret_message
   ```

5. Submit the exact flag printed in the terminal.

### Troubleshooting

If the file is missing, confirm the current directory with `pwd`, then list the intended directory directly with `ls -la /home/mcskidy/`. Keep the leading dot in `.secret_message`.

**Answer:** `THM{TrustNoBunny}`

## Challenge 6: The Leak in the List

**Question:** “What's the flag?”

### Objective

Use the simulated breach checker to determine whether McSkidy's email address appears in a breach and finish the exercise.

### Steps

1. Open Challenge 6 and click **View Site**.
2. Enter the exact address:

   ```text
   mcskidy@tbfc.com
   ```

3. Submit the lookup and wait for the domain results.
4. Review each listed domain and its status. Identify the entry explicitly marked **Compromised**.
5. Follow the challenge's selection/confirmation flow. The flag appears after the result is correctly handled.

### Why it matters

A breach lookup can indicate that an address appears in known leaked data, but a match does not necessarily prove the account is currently compromised. In a real incident, change any affected password, eliminate password reuse, enable MFA, review login history, and revoke suspicious sessions. Do not submit someone else's private address to an external service without a legitimate reason.

**Answer:** `THM{LeakedAndFound}`

## Challenge 7: WiFi Woes in Wareville

**Question:** “What's the flag?”

### Objective

Log in to the simulated router using its default credentials and replace them with a strong password.

### Steps

1. Open Challenge 7 and click **View Site**.
2. On the simulated router login page, use the default credentials specified by the task:
   - Username: `admin`
   - Password: `admin`
3. After login, open **Security Settings**.
4. Find the router administrator password setting.
5. Choose a unique password that satisfies the interface's validation checks, then save it.
6. Confirm the simulator accepts the change and submit/finish the task as directed.

### Why it works

Default credentials are publicly known and are routinely tried by attackers. Changing the router administrator password prevents simple default-login access. This is separate from changing the Wi-Fi network's SSID or wireless passphrase.

### Defensive checklist

Change default administrator credentials, use a long unique password, update router firmware, disable remote administration when unnecessary, configure WPA3 or properly configured WPA2-AES, and isolate guest devices.

**Answer:** `THM{NoMoreDefault}`

## Challenge 8: The App Trap

**Question:** “What's the flag?”

### Objective

Review connected third-party applications and revoke access for the app with suspiciously broad permissions.

### Steps

1. Open Challenge 8 and click **View Site**.
2. Inspect the simulated account's list of connected apps.
3. Open the details of suspicious-looking apps and review each permission. Do not choose by name alone.
4. Look for permissions unrelated to an application's purpose, especially access to highly sensitive information such as a **password vault**.
5. Select the app whose permission scope matches the scenario's suspicious behaviour.
6. Click **Revoke Access** and confirm the action if prompted.
7. Submit or finish the task as directed; the simulator displays the flag.

### Why it works

Third-party integrations can receive ongoing delegated access. Revoking the grant blocks future access through that connection, but it may not remove data the app already copied. In real environments, investigate the grant, review access logs, rotate potentially exposed secrets, and revoke the app's sessions/tokens.

**Answer:** `THM{AppTrapped}`

## Challenge 9: The Chatbot Confession

**Question:** “What's the flag?”

### Objective

Identify messages from FestiveBot that disclose sensitive information.

### Steps

1. Open Challenge 9 and click **View Site**.
2. Read the entire conversation before selecting messages.
3. Identify messages that expose private or internal data, such as:
   - Passwords, tokens, or authentication secrets.
   - Internal-only URLs or service locations.
   - Company information that should not be disclosed to the current user.
4. Select each message containing sensitive information; leave ordinary, non-sensitive messages unselected.
5. Submit the selection. If it is rejected, review each message again and ensure no secret-bearing message was missed or a harmless message included.

### Why it matters

An AI assistant should only receive the data it needs. Prompt instructions alone cannot guarantee confidentiality. Applications should enforce data permissions before retrieval, avoid placing secrets in prompts, restrict tools, validate outputs, and log risky actions without recording unnecessary sensitive content.

**Answer:** `THM{DontFeedTheBot}`

## Challenge 10: The Bunny's Browser Trail

**Question:** “What's the flag?”

### Objective

Inspect HTTP log entries and identify the unusual user-agent string.

### Steps

1. Open Challenge 10 and click **View Site**.
2. Read the HTTP log entries in the simulator.
3. Find the **User-Agent** field for each request and compare the values with ordinary browser identifiers such as Chrome, Firefox, and Edge.
4. The unusual string called out by the scenario is:

   ```text
   BunnyOS/1.0 (HopSecBot)
   ```

5. Select the matching suspicious log entry and submit.

### Why it works

A user-agent string describes the client software but is supplied by the client and can be forged. In a real investigation, correlate it with request frequency, paths, status codes, source IP, authentication events, and other telemetry rather than treating the string alone as definitive proof of malicious activity.

**Answer:** `THM{EastmasIsComing}`

## Complete answer list

| Challenge | Exact question | Answer |
|---|---|---|
| 1. Password Pandemonium | What's the flag? | `THM{StrongStart}` |
| 2. The Suspicious Chocolate.exe | What's the flag? | `THM{NotSoSweet}` |
| 3. Welcome to the AttackBox! | What's the flag? | `THM{Ready2Hack}` |
| 4. The CMD Conundrum | What's the flag? | `THM{WhereIsMcSkidy}` |
| 5. Linux Lore | What's the flag? | `THM{TrustNoBunny}` |
| 6. The Leak in the List | What's the flag? | `THM{LeakedAndFound}` |
| 7. WiFi Woes in Wareville | What's the flag? | `THM{NoMoreDefault}` |
| 8. The App Trap | What's the flag? | `THM{AppTrapped}` |
| 9. The Chatbot Confession | What's the flag? | `THM{DontFeedTheBot}` |
| 10. The Bunny's Browser Trail | What's the flag? | `THM{EastmasIsComing}` |

## Phase 5: Key Takeaways & Remediation

- Use unique passwords and check for exposure to known breaches.
- Treat suspicious executable files as untrusted and base triage on evidence, not filenames.
- Learn basic Linux and Windows commands for inspecting files and directories.
- Hidden files are not access control; use correct permissions and protect sensitive data.
- Replace default router credentials and maintain firmware.
- Review third-party application permissions and revoke unneeded grants.
- Prevent AI applications from revealing secrets through permission checks, data minimization, and constrained tools.
- Investigate logs using multiple fields; a user-agent string alone can be spoofed.

## Reference

Official room: https://tryhackme.com/room/adventofcyberpreptrack

The interactive room remains authoritative if the task UI or generated values change.
