# Advent of Cyber Prep Track 2025: Complete Walkthrough

Official room: https://tryhackme.com/room/adventofcyberpreptrack

## Room Overview & Information

**Platform:** TryHackMe  
**Event:** Advent of Cyber 2025 preparation  
**Format:** Ten short, interactive beginner challenges  
**Topics:** Password hygiene, malware triage, Linux and Windows command-line basics, breach checking, wireless-router security, third-party app permissions, AI data disclosure, and web-log investigation.

The official room says each challenge opens with **View Site** and each challenge has its own interactive page. Click View Site again when changing tasks. The tasks are simulated learning exercises, not permission to test real accounts, networks, or applications.

The flag value for each challenge is displayed by its own interactive site after the objective is completed. This write-up records the exact task, the detailed solving steps, and the verification method. It does not invent flag strings that have not been captured from the live challenge.

## Before you start

1. Open the official room and read the current task instructions.
2. Click View Site for the current challenge.
3. Use the task's simulated inputs and dummy data only.
4. Follow the completion feedback, then copy the flag exactly as displayed.
5. Reopen View Site when moving to another challenge; the page changes between tasks.
6. Keep a note of the input, result, and lesson. Do not reuse any challenge password outside the lab.

## Challenge 1: Password Pandemonium

**Question:** “What's the flag?”

### Objective

Create a password that passes the application's policy and is not found in its simulated leaked-password database.

### Steps

1. Open Challenge 1 and click View Site.
2. Read the checks shown by the password validator.
3. Enter a password with at least 12 characters.
4. Include uppercase letters, lowercase letters, numbers, and symbols as required.
5. Make the password unique and avoid known examples such as the weak password shown in the scenario.
6. Submit it. If a check fails, read the message and correct that requirement rather than changing random characters.
7. Once the application accepts the password and indicates it is not on the simulated breach list, copy the flag it displays.

### Why it matters

A strong password should be long, unique, and not present in common or breached-password lists. A single substitution in a familiar pattern is not a reliable substitute for uniqueness. Use a password manager for real accounts, and never reuse a challenge password.

**Flag:** Retrieve the value from the active challenge after all checks pass.

## Challenge 2: The Suspicious Chocolate.exe

**Question:** “What's the flag?”

### Objective

Use the simulated malware-scanning interface to classify the supplied file.

### Steps

1. Open Challenge 2 and click View Site.
2. Select the scan action for the supplied file, chocolate.exe.
3. Wait for the scanner's report.
4. Inspect the counts, verdicts, detection name, vendor, and reference shown by the interface.
5. Read the entire result instead of deciding from the filename alone.
6. Choose the classification supported by the room's report and submit it.
7. Copy the flag after the challenge confirms success.

### How to interpret the scan

One malicious detection among many clean results can be a false positive in real investigations. In this simulated challenge, follow the supplied evidence and expected task flow. In a real malware triage, also record the SHA-256 hash, source, file type, sandbox behaviour, network indicators, and vendor-report context. A filename or icon is not proof that a file is safe.

**Flag:** Retrieve the value shown by the active simulator after the correct classification.

## Challenge 3: Welcome to the AttackBox!

**Question:** “What's the flag?”

### Objective

Locate and read the welcome message in the Linux challenge workspace.

### Steps

Open the AttackBox terminal supplied by the exercise. Begin by checking the current directory and its entries:

    pwd
    ls -la

- pwd prints the current working directory.
- ls -la lists normal and hidden entries with details.

Move into the challenge directory:

    cd challenges/
    ls -la

Read the file:

    cat welcome.txt

- cd changes directories.
- cat writes the file's contents to the terminal.

Submit the flag that the file prints.

### Troubleshooting

If challenges/ is not found, use pwd and ls -la to establish where the terminal started. Linux paths are case-sensitive. Do not assume that the same directory is present outside the provided exercise environment.

**Flag:** Copy the exact value from welcome.txt.

## Challenge 4: The CMD Conundrum

**Question:** “What's the flag?”

### Objective

Use Windows Command Prompt to find and read a hidden file.

### Steps

Open Challenge 4 and use the supplied Windows terminal.

List ordinary entries:

    dir

Then include entries with hidden or system attributes:

    dir /a

Move into the folder named by the challenge:

    cd mystery_data
    dir /a

Read the file:

    type hidden_flag.txt

If you are still in the parent directory, use the relative path:

    type mystery_data\hidden_flag.txt

The /a switch includes entries regardless of their attributes. The type command prints a text file. A hidden file is not encrypted; its attribute only affects how it is listed by default.

**Flag:** Copy the exact value printed by the file.

## Challenge 5: Linux Lore

**Question:** “What's the flag?”

### Objective

Locate McSkidy's hidden message in the Linux home directory.

### Steps

Open Challenge 5 and use the provided terminal.

Change into the specified home directory:

    cd /home/mcskidy/

List all files, including dotfiles:

    ls -la

Read the hidden message:

    cat .secret_message

Linux filenames beginning with a dot are usually omitted by a basic ls listing. The -a option reveals them. Keep the leading dot in .secret_message when entering the path.

If the file is not shown, check pwd and inspect the intended path directly:

    pwd
    ls -la /home/mcskidy/

**Flag:** Copy the exact value from the hidden message file.

## Challenge 6: The Leak in the List

**Question:** “What's the flag?”

### Objective

Use the room's simulated breach checker to determine the status of the supplied account address.

### Steps

1. Open Challenge 6 and click View Site.
2. Enter the exact address specified by the room, shown in the current exercise as mcskidy@tbfc.com.
3. Submit the lookup and wait for the result.
4. Review the returned domains and their status.
5. Identify the entry marked as compromised, then follow the simulator's confirmation flow.
6. Copy the flag after the task reports success.

### Security lesson

A breach lookup can show that an address appears in known leaked data, but it does not by itself prove the current account is compromised. In a real incident, change affected passwords, eliminate reuse, enable MFA, review authentication activity, and revoke suspicious sessions. Do not submit someone else's private address to a third-party service without a legitimate reason.

**Flag:** Obtain the value from the active challenge after completing the lookup.

## Challenge 7: WiFi Woes in Wareville

**Question:** “What's the flag?”

### Objective

Secure the simulated router by replacing its default administrator credentials.

### Steps

1. Open Challenge 7 and click View Site.
2. Use the default login details provided in the simulation. The documented scenario starts with username admin and password admin.
3. After login, navigate to Security Settings.
4. Locate the router administrator-password field.
5. Choose a strong, unique password that satisfies the interface's checks.
6. Save the change and verify the simulator confirms it.
7. Complete the task flow and copy the flag.

### Security lesson

Default credentials are publicly known and commonly tried by attackers. Change the administrator password, update the firmware, disable remote administration when unnecessary, enable WPA3 or properly configured WPA2-AES, and isolate guest devices. The router administrator password is distinct from the wireless network's passphrase.

**Flag:** Copy it from the simulator after the password change is confirmed.

## Challenge 8: The App Trap

**Question:** “What's the flag?”

### Objective

Review connected third-party applications and revoke access for the app with suspiciously broad permissions.

### Steps

1. Open Challenge 8 and click View Site.
2. Inspect the list of connected apps.
3. Open each relevant app's details and review the permissions it has been granted.
4. Compare the permission scope with the app's purpose. Pay particular attention to access to sensitive data such as a password vault.
5. Select the app whose granted permissions match the suspicious scenario.
6. Choose Revoke Access and confirm the action if prompted.
7. Verify that the simulator reports the permission was revoked, then copy the flag.

### Security lesson

Revoking a grant blocks future access through that connection, but it may not erase data the third-party app copied earlier. In a real incident, investigate the grant, review access logs, rotate possibly exposed secrets, and revoke sessions or tokens as needed.

**Flag:** Copy the exact value after the simulator confirms success.

## Challenge 9: The Chatbot Confession

**Question:** “What's the flag?”

### Objective

Identify messages from FestiveBot that disclose information which should remain private.

### Steps

1. Open Challenge 9 and click View Site.
2. Read the full conversation before selecting messages.
3. Select messages that reveal sensitive material, such as authentication secrets, internal-only URLs, or company information that should not be disclosed to the current user.
4. Do not select ordinary messages simply because they sound technical.
5. Submit the selection and inspect the feedback.
6. If the selection is rejected, review each message again and identify what information it reveals.
7. Copy the flag after the exercise confirms that the disclosure messages were identified correctly.

### Security lesson

An AI assistant should receive only the data it needs. Prompt instructions alone cannot guarantee confidentiality. Enforce permissions before retrieving private data, restrict tools, validate responses, and avoid placing secrets in prompts or logs. Validate whether a backend action occurred instead of trusting the chatbot's statement.

**Flag:** Obtain the value shown after the message-selection task is passed.

## Challenge 10: The Bunny's Browser Trail

**Question:** “What's the flag?”

### Objective

Inspect HTTP logs and identify the unusual User-Agent value.

### Steps

1. Open Challenge 10 and click View Site.
2. Read the HTTP log entries.
3. Locate the User-Agent field for each request.
4. Compare the values with expected browser identifiers and identify the outlier.
5. In the documented scenario, the unusual value is BunnyOS/1.0 (HopSecBot).
6. Select the entry containing that value and submit.
7. Copy the flag after the simulator confirms success.

### Security lesson

A User-Agent string is supplied by the client and can be forged. Correlate it with request frequency, paths, status codes, source IP, authentication events, and other telemetry before deciding that a request is malicious.

**Flag:** Copy the value displayed by the current challenge.

## Complete task checklist

| Challenge | Exact question | Verification method |
|---|---|---|
| Password Pandemonium | What's the flag? | Satisfy all password-policy and breach checks |
| The Suspicious Chocolate.exe | What's the flag? | Read the scan report and choose the supported verdict |
| Welcome to the AttackBox! | What's the flag? | Read the file under the challenges directory |
| The CMD Conundrum | What's the flag? | Reveal and read the hidden Windows file |
| Linux Lore | What's the flag? | Read the dotfile in McSkidy's home directory |
| The Leak in the List | What's the flag? | Complete the simulated breach lookup and status confirmation |
| WiFi Woes in Wareville | What's the flag? | Change the default router administrator password |
| The App Trap | What's the flag? | Revoke the connection with suspiciously broad permissions |
| The Chatbot Confession | What's the flag? | Select the messages disclosing sensitive information |
| The Bunny's Browser Trail | What's the flag? | Select the HTTP log entry with the suspicious User-Agent |

## Final remediation checklist

- Use unique passwords and block common or breached passwords.
- Treat executable files as untrusted until evidence supports a verdict.
- Learn both Linux and Windows file-discovery commands.
- Remember that hidden files are not access control.
- Replace router defaults and keep firmware current.
- Review third-party application grants and revoke unneeded access.
- Prevent AI applications from retrieving or disclosing data beyond the user's permissions.
- Investigate logs using multiple fields because user-agent strings can be forged.

The official room's interactive pages remain the source of truth for each flag. These steps explain how to reach and verify each answer without publishing guessed flag values.
