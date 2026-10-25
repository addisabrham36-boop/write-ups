# Road: TryHackMe Walkthrough

Room: https://tryhackme.com/room/road

Road is a medium Linux boot-to-root machine. The intended chain demonstrates broken access control in a password-change function, an unrestricted profile-image upload, credential discovery through MongoDB, and a sudo configuration that preserves LD_PRELOAD for a backup utility. The user and root flags are not reproduced here.

## 1. Scan and enumerate

Start the target and record its current IP. The machine commonly exposes SSH and HTTP:

    nmap -sC -sV -oN road-nmap.txt TARGET_IP

Enumerate web paths:

    gobuster dir -u http://TARGET_IP/ -w /path/to/wordlist.txt

Use a wordlist available in the AttackBox and inspect the results. The application is under /v2. Browse to it, register a normal lab account, and examine the dashboard and profile functions. Do not reuse a target IP or credential from another person's walkthrough.

## 2. Find the broken access-control flaw

The profile interface indicates that only the administrator can upload a profile picture and reveals the administrator's email address. A password-change or account-reset function accepts the target email as a request parameter.

Intercept your own password-change request with Burp Suite. Inspect the request body and identify the email field and new-password fields. In the authorised lab, change only the email value to the administrator address shown by the application, forward the request, and inspect the response.

The server should have checked that the authenticated user was authorised to change the account identified by that email. It does not. After confirming that the change succeeded, log in through the normal web interface with the new lab password. This is an IDOR/broken-access-control issue, not a cryptographic break.

## 3. Upload a PHP shell through the profile image feature

The admin account can access the profile-image upload function. The application accepts a PHP file despite presenting the feature as an image upload. In the lab, use a standard reverse-shell test file from the AttackBox, edit its callback address and port to match your own lab connection, and upload it using the profile form.

Start a listener on the chosen port, for example:

    nc -lvnp 4444

The page source contains a comment revealing that uploaded profile images are stored under /v2/profileimages/. Visit the uploaded PHP filename at that path to trigger it. If the listener receives a connection, verify the shell's identity and host before continuing.

Use only the isolated TryHackMe machine. Do not upload a shell to a real service or expose a listener to the public internet.

## 4. Confirm the initial foothold

The reverse shell runs as the web-server account, commonly www-data. Verify this rather than assuming the upload gave administrator access:

    whoami
    id
    hostname
    pwd

The user flag is in the webdeveloper user's home directory in the intended path. Read it on your own target and submit it to the room; the value is omitted from this public note.

## 5. Find the webdeveloper credentials

Enumerate the system from the shell. Check listening services, application configuration, and files readable by the current account. MongoDB is running locally and stores application data. If the room's evidence indicates that it permits local unauthenticated access, connect to the local database using the client installed on the target.

Typical MongoDB shell commands include:

    show dbs
    use DATABASE_NAME
    show collections
    db.COLLECTION_NAME.find()

Replace the database and collection placeholders with names discovered on the target. Inspect the actual documents and identify the webdeveloper credential supplied by the lab. Do not copy unrelated personal data or credentials into the public write-up.

Use the recovered credential to switch to webdeveloper or establish SSH access if the target and room allow it. Verify the identity after switching accounts.

## 6. Inspect sudo permissions

As webdeveloper, run:

    sudo -l

The intended configuration allows the user to run /usr/bin/sky_backup_utility with elevated privileges and preserves the LD_PRELOAD environment variable. LD_PRELOAD tells the dynamic linker to load a specified shared library before other libraries. Preserving it for a privileged command can allow an attacker to run code with the command's elevated privileges.

This is a serious configuration flaw. A privileged utility should not inherit an attacker-controlled library path.

## 7. Demonstrate the LD_PRELOAD escalation in the lab

Create a small C shared library in the lab workspace that runs a shell with the effective privileges of the privileged process. Compile it as a shared object using the room's supported compiler:

    gcc -fPIC -shared -o exploit.so exploit.c -nostartfiles

Transfer or create the file on the target where webdeveloper can read it. Then invoke the allowed utility with LD_PRELOAD pointing to the library:

    sudo LD_PRELOAD=/path/to/exploit.so /usr/bin/sky_backup_utility

The exact library source and path must match your lab environment. Do not use this on a non-lab machine. If it succeeds, verify the resulting identity with whoami and id before accessing root-owned files.

## 8. Remediation

- Enforce account ownership checks on every password-change request.
- Do not accept arbitrary account identifiers from the client without authorisation.
- Validate uploaded content by type and content, store uploads outside executable web paths, and disable execution in upload directories.
- Restrict database access and require authentication.
- Avoid storing plaintext credentials in application databases.
- Remove unnecessary sudo permissions and never preserve LD_PRELOAD for a privileged command.
- Run the web application with least privilege and monitor unexpected child processes.

## What I took from this room

Road demonstrates how several ordinary flaws can chain into full compromise. The password-change endpoint crosses an authorisation boundary, the upload feature crosses from file storage into code execution, database exposure reveals another account's credentials, and LD_PRELOAD turns a privileged utility into root access. Each stage needs its own fix.

## Phase 1 continued: Web and database enumeration

The path /v2 is the key web-application clue in this room. Inspect visible features after registering a normal lab user and preserve a baseline request before modifying it. A service scan and focused directory discovery help identify the app without guessing endpoints:

```bash
nmap -sC -sV -oN road-nmap.txt TARGET_IP
gobuster dir -u http://TARGET_IP/ -w /path/to/wordlist.txt -o road-web.txt
```

Use a wordlist installed in the AttackBox. Confirm the actual path and response before treating a result as the admin panel.

## Phase 2 continued: Confirm the broken access control

Capture the ordinary password-change request in Burp Suite. Record the request method, route, session cookie, account identifier, and new-password fields. Change only the identifier to the administrator address exposed by the application, then send the request inside the lab.

The critical evidence is whether the server changes the targeted account's password despite the request originating from another account. A 200 response is not enough; use the normal login form to verify the resulting account context. This is a missing server-side authorization check, commonly described as broken access control or IDOR depending on the exact design.

## Phase 2 continued: Upload handling

The profile-image form should accept images only, but the vulnerable application permits a PHP file to be stored in a web-accessible location where the server executes it. This combines weak server-side validation with dangerous file placement.

Use only the room's test file and callback address. Before triggering it, start the listener:

```bash
nc -lvnp 4444
```

If the callback arrives, establish the initial context:

```bash
whoami
id
hostname
pwd
```

The shell typically runs as the web-server account, not as root. Restore or remove the uploaded proof file when the lab permits cleanup.

## Phase 3: MongoDB and sudo configuration

From the shell, check the application's configuration and local listening ports rather than immediately scanning the entire network:

```bash
ss -lntup
find /var/www -maxdepth 4 -type f -readable 2>/dev/null
```

Review relevant configuration files for database connection settings. If MongoDB is reachable locally and the room permits it, inspect the specific application database for stored credentials. Do not dump unrelated collections or data.

The intended later escalation depends on a sudo rule for a backup utility that preserves the LD_PRELOAD environment variable. Inspect the current policy:

```bash
sudo -l
```

A preserved LD_PRELOAD can cause a privileged process to load a shared library from a user-controlled location, unless the environment is sanitized and the binary is protected. Verify the exact permitted command and environment setting first. Use only the proof of concept provided or directed by the room, and validate root access with id.

## Phase 4: Flag workflow

Read the user flag after locating it in the intended webdeveloper home path. Obtain the root flag only after the sudo escalation is verified. Use the live target values; this section does not invent flags or assume the current deployment matches an older run.

## Phase 5: Remediation

- Enforce authorization on password changes using the authenticated account, not a client-supplied email alone.
- Validate uploaded file content and store uploads outside executable paths.
- Run the web application as a dedicated unprivileged service account.
- Protect database credentials and restrict database listeners.
- Remove unnecessary sudo permissions and never preserve attacker-controlled environment variables for privileged programs.
- Review the application for secrets, disable debugging in production, and monitor account changes and file uploads.
