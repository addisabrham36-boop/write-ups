# Hydra: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/hydra

## Room Overview & Information

**Category:** Authentication testing  
**Difficulty:** Beginner  
**Focus:** Using Hydra to test SSH and HTTP form authentication against the room's deliberately vulnerable machine.

Hydra is an online authentication testing tool. It tries candidate credentials against a service and reports responses that appear to succeed. It is not the same as offline password cracking: each candidate is sent to the target service, so attempts can be logged, rate-limited, or trigger account lockouts.

Use Hydra only against the machine deployed by TryHackMe or another system you have explicit permission to test.

## Task 1: Hydra Introduction

Hydra supports multiple protocols, including SSH, FTP, HTTP form logins, SMTP, SMB, and others. The command must match the service and authentication method. A correct wordlist is not enough if the username, endpoint, request format, or failure condition is wrong.

The core options used in this room are:

| Option | Meaning |
|---|---|
| `-l USERNAME` | Test one username |
| `-L USER_FILE` | Read usernames from a file |
| `-p PASSWORD` | Test one password |
| `-P WORDLIST` | Read passwords from a file |
| `-t N` | Use N parallel tasks/connections |
| `-V` | Show each login attempt |
| `-vV` | Increase verbosity |
| `-s PORT` | Specify a non-default port |
| `-f` | Stop after the first successful credential pair for the target |

Use a modest thread count for the room. Too many parallel attempts can overload the service or cause the results to be unreliable.

## Task 2: Connect to the machine

Start the AttackBox and the lab machine. Wait for the target to finish booting, then open the web service at:

```text
http://MACHINE_IP
```

Replace `MACHINE_IP` with the current address shown by TryHackMe. Do not use a remembered address from another session.

Hydra is already installed on the AttackBox. On another Debian-based lab system, it may be installed with the package manager, but use a lab-provided tool environment where possible.

Check the target's services with Nmap:

```bash
nmap -sC -sV -oN hydra-nmap.txt MACHINE_IP
```

- `-sC` runs the default scripts.
- `-sV` attempts to identify service versions.
- `-oN` saves the output.

The room's exercises focus on a web form and SSH. Confirm which ports and services are available on your current instance.

## Phase 1: Brute-force the web login

### 1. Inspect the login form first

Open the website in the browser and identify:
- The form path.
- Whether it submits with GET or POST.
- The username field name.
- The password field name.
- The exact failure message returned for an invalid login.

Use browser Developer Tools → **Network** to inspect the request, or use Burp Suite Proxy to capture it. This step matters because Hydra must reproduce the form's request accurately.

### 2. Understand the HTTP POST form syntax

A typical Hydra command for a POST form looks like this:

```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt MACHINE_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

This is an example of the syntax, not a claim that the exact failure text is identical for every target. Match the form fields and failure condition to the actual room response.

Parameter breakdown:
- `hydra` launches the tool.
- `-l molly` supplies the username `molly`, which is the username used in this room.
- `-P /usr/share/wordlists/rockyou.txt` loads candidate passwords from the wordlist.
- `MACHINE_IP` identifies the deployed target.
- `http-post-form` tells Hydra to submit a POST form.
- `/` is the login path in this example.
- `username=^USER^&password=^PASS^` describes the form body. Hydra replaces `^USER^` and `^PASS^` with the selected username and candidate password.
- `F=incorrect` tells Hydra that the response containing the specified failure string indicates a failed login.
- `-V` prints each attempt.

If the real form uses different field names or a different failure message, the example must be adjusted. Do not guess the failure condition: test one invalid login and inspect the response first.

### 3. Verify the web password

When Hydra reports a candidate, log in through the browser to verify it. Confirm that the application accepts the credential and displays the expected account page. A response match can be a false positive if the failure condition was configured incorrectly.

### Question 1: “Use Hydra to brute-force molly's web password. What is the value of flag 1?”

After the web login succeeds:
1. Open the web application with the recovered lab credential.
2. Locate the flag in the account page or dashboard.
3. Submit the flag to the active TryHackMe room.
4. Redact it as `THM{hidden}` in public notes.

Do not publish the password or flag from your lab instance.

## Phase 2: Brute-force SSH

### 4. Understand the SSH command

For SSH, the room's command pattern is:

```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt -t 4 MACHINE_IP ssh
```

Parameter breakdown:
- `-l molly` selects the username.
- `-P` selects the password list.
- `-t 4` uses four parallel tasks.
- `MACHINE_IP` is the target.
- `ssh` selects the SSH protocol module.

The exact wordlist path can vary. Confirm that the file exists:

```bash
ls -l /usr/share/wordlists/rockyou.txt
```

Some environments provide a compressed `rockyou.txt.gz`. If the room's file is compressed, use the room's instructions or decompress a local copy before running Hydra. Do not alter the original wordlist unnecessarily.

### 5. Interpret the result

Hydra may print a line containing the service, username, and candidate password when it believes authentication succeeded. Confirm the result by connecting with SSH:

```bash
ssh molly@MACHINE_IP
```

Enter the candidate password when prompted. If login succeeds, verify the account and inspect the home directory:

```bash
whoami
id
pwd
ls -la
```

These commands confirm the account, its groups, current directory, and available files. Do not assume the SSH password is the same as the web password; the room tests the two services separately.

### Question 2: “Use Hydra to brute-force molly's SSH password. What is the value of flag 2?”

1. Use Hydra against the room's SSH service with the username and wordlist specified by the task.
2. Verify the candidate by logging in over SSH.
3. Locate the second flag using the room's instructions.
4. Submit the value to TryHackMe and redact it in the public write-up.

## Troubleshooting

- **No valid password found:** verify the username, wordlist path, service port, and target address.
- **Hydra reports too many errors:** reduce `-t`, check network stability, and confirm the service is reachable.
- **Web form never succeeds:** check GET versus POST, field names, URL path, cookies, CSRF requirements, and the failure string.
- **False positive:** verify the candidate manually; a misconfigured failure condition can make every response look successful.
- **SSH connection refused:** confirm SSH is listening on the target and that the machine has completed booting.
- **Wordlist missing:** inspect the AttackBox's wordlist directory and follow the room's download instructions.

## Phase 3: Defences and remediation

- Require long, unique passwords and prevent common or breached passwords.
- Enable MFA where practical.
- Rate-limit failed login attempts and detect distributed guessing.
- Avoid verbose errors that disclose account existence.
- Monitor repeated failures and successful logins following a burst of failures.
- Use secure password-reset flows and protect session tokens.
- Disable unused authentication methods and restrict management services to trusted networks.

## Key takeaways

Hydra automates online authentication tests, but it only works reliably when the protocol, form structure, candidate list, and failure condition are understood. Verify every reported credential through the intended service and keep all attempts inside the authorised lab.