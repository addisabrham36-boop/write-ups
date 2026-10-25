# Hydra: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/hydra

## Room Overview & Information

**Category:** Authentication testing  
**Difficulty:** Beginner  
**Focus:** Using Hydra to test SSH and HTTP form authentication against the room's deliberately vulnerable machine.

Hydra is an online authentication testing tool. It sends candidate credentials to a service and reports responses that appear to succeed. This differs from offline password cracking: each candidate is sent to the target, so attempts can be logged, rate-limited, or trigger account lockouts.

Use Hydra only against the machine deployed by TryHackMe or another system you are explicitly authorised to test.

## Task 1: Hydra Introduction

Hydra supports protocols including SSH, FTP, HTTP form logins, SMTP, SMB, and others. The command must match the service and authentication method. A correct wordlist is not enough if the username, endpoint, request format, or failure condition is wrong.

### Common options

| Option | Meaning |
|---|---|
| `-l USERNAME` | Test one username |
| `-L USER_FILE` | Read usernames from a file |
| `-p PASSWORD` | Test one password |
| `-P WORDLIST` | Read passwords from a file |
| `-t N` | Use N parallel tasks/connections |
| `-V` | Show each login attempt |
| `-s PORT` | Specify a non-default port |
| `-f` | Stop after the first successful credential pair for the target |

## Task 2: Deploy the target

Start the AttackBox and lab machine. Wait for the target to finish booting, then open the web service using the IP address displayed by TryHackMe. The machine may take a few minutes to become ready.

Run a service scan and save the results:

\`\`\`bash
nmap -sC -sV -oN hydra-nmap.txt MACHINE_IP
\`\`\`

- **-sC** runs the default scripts.
- **-sV** attempts to identify service versions.
- **-oN** saves the output.

The room's exercises focus on a web form and SSH. Confirm which services are available on your current instance.

## Phase 1: Brute-force the web login

### 1. Inspect the login request

Open the website and identify the login path, request method, username field, password field, and failure message. Use browser Developer Tools → Network or Burp Suite Proxy to capture the request.

The form uses a POST request to `/login`. The username and password fields are named `username` and `password`. The failure response contains the word `incorrect`, which Hydra can use to distinguish failed attempts.

### 2. Run Hydra against the POST form

\`\`\`bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt MACHINE_IP http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V
\`\`\`

Parameter breakdown:
- **-l molly** supplies the username.
- **-P** selects the password wordlist.
- **MACHINE_IP** is the current target address.
- **http-post-form** selects the HTTP POST form module.
- **/login** is the form endpoint.
- **username=^USER^&password=^PASS^** describes the form body; Hydra replaces the placeholders with each candidate.
- **F=incorrect** tells Hydra that a response containing this failure string means the login failed.
- **-V** prints each attempt.

The wordlist path may vary. If RockYou is compressed, follow the room's instructions to make it available. The failure string must match the actual application response; if it is wrong, Hydra can report false positives.

### 3. Verify the web credential

The room's web password for the fictional user Molly is:

**Password:** `sunshine`.

Verify it by logging in through the browser. A tool result is not enough by itself; confirm that the site accepts the credential and displays the expected account page.

### Question: “Use Hydra to bruteforce molly's web password. What is flag 1?”

1. Run the form attack against the current lab address.
2. Confirm the candidate password in the browser.
3. Locate the first flag in the account page or dashboard.
4. Submit the value shown by the active room.

The flag is redacted here as `THM{hidden}`.

## Phase 2: Brute-force SSH

### 4. Run Hydra against SSH

\`\`\`bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt -t 4 MACHINE_IP ssh
\`\`\`

- **-l molly** selects the username.
- **-P** selects the password list.
- **-t 4** uses four parallel tasks.
- **MACHINE_IP** is the target.
- **ssh** selects the SSH module.

The room's SSH password for Molly is:

**Password:** `butterfly`.

Verify it by connecting to the target:

\`\`\`bash
ssh molly@MACHINE_IP
\`\`\`

After login, verify the identity and inspect the home directory:

\`\`\`bash
whoami
id
pwd
ls -la
\`\`\`

### Question: “Use Hydra to bruteforce molly's SSH password. What is flag 2?”

1. Run the SSH attack using the room's username and wordlist.
2. Verify the recovered password through SSH.
3. Locate the second flag in the user's home directory as directed by the room.
4. Submit the value shown by the active machine.

The second flag is redacted here as `THM{hidden}`.

## Troubleshooting

- **No valid password found:** verify the username, wordlist path, service port, and target address.
- **Hydra reports errors:** reduce the thread count, check network stability, and confirm the service is reachable.
- **Web form never succeeds:** check GET versus POST, field names, endpoint path, cookies, CSRF requirements, and the failure string.
- **False positive:** verify the candidate manually; a wrong failure condition can make every response look successful.
- **SSH connection refused:** confirm SSH is listening and the machine has completed booting.
- **Wordlist missing:** inspect the AttackBox's wordlist directory and follow the room's instructions.

## Defences and remediation

- Require long, unique passwords and prevent common or breached passwords.
- Enable MFA where practical.
- Rate-limit failed login attempts and monitor distributed guessing.
- Avoid verbose errors that disclose account existence.
- Monitor repeated failures and successful logins following a burst of failures.
- Use secure password-reset flows and protect session tokens.
- Disable unused authentication methods and restrict management services to trusted networks.

## Key takeaways

Hydra automates online authentication tests, but it only works reliably when the protocol, form structure, candidate list, and failure condition are understood. Verify every reported credential through the intended service, and keep all attempts inside the authorised lab.