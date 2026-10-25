# GamingServer: TryHackMe Walkthrough

Room: https://tryhackme.com/room/gamingserver

GamingServer is a beginner boot-to-root challenge. The goal is to enumerate a deliberately vulnerable host, find a route to a normal user account, and then investigate whether that account can obtain elevated privileges. The exact IP and any discovered credentials are specific to the running instance.

## 1. Start the machine and scan it

Start the lab and record the assigned target IP. Scan the target and save the result:

    nmap -sC -sV -oN gamingserver-nmap.txt TARGET_IP

Then scan all TCP ports if the initial results do not explain the exposed services:

    nmap -p- --min-rate 1000 -oN gamingserver-allports.txt TARGET_IP

Use TARGET_IP as a placeholder for the live address. Review open ports and service versions, then run a focused scan against the ports that matter. Do not assume a service is vulnerable merely because its version looks old.

## 2. Enumerate the web server

Open the web service in a browser. Inspect the page source, links, comments, robots.txt, and any paths referenced by the application. Record interesting files and directories.

If the room requires directory discovery, a general pattern is:

    gobuster dir -u http://TARGET_IP/ -w /path/to/wordlist.txt

Use a wordlist that actually exists in the AttackBox. Check the response codes and manually inspect promising results. A discovered directory is only a lead; confirm what it contains and whether it exposes useful information.

## 3. Look for unintended information disclosure

Begin with low-impact checks: publicly exposed files, source comments, backup files, configuration fragments, and documents that the application links to. If a file appears to contain a credential or private key, handle it carefully and use it only as the room authorises.

Do not guess a username or password from a generic walkthrough. Build a candidate only from evidence found in this instance, then test it against the service indicated by the room.

## 4. Establish user access

Once the evidence points to a valid route into the machine, use the service and credentials exactly as the room permits. If SSH is exposed, a normal connection uses the form:

    ssh USERNAME@TARGET_IP

Replace USERNAME and TARGET_IP with values found in the lab. If the key or password is rejected, re-check the username, key permissions, and whether the credential belongs to that service. Do not repeatedly try unrelated credentials.

After login, confirm the account identity and inspect the home directory:

    whoami
    id
    pwd
    ls -la

Find the user flag only where the room expects it. In a public write-up, the flag value is intentionally omitted; obtain and submit it from your own live instance.

## 5. Enumerate privilege escalation opportunities

Once logged in, gather basic system information:

    hostname
    uname -a
    sudo -l
    find / -perm -4000 -type f 2>/dev/null

These commands identify the host and kernel, show permitted sudo commands when allowed, and list files with the SUID bit. The output is machine-specific. Do not run an exploit just because a binary appears in a list; verify its permissions, version, and known behaviour first.

Also inspect scheduled tasks, writable scripts, service configuration, and file ownership where the room points you. A valid escalation path should connect a specific misconfiguration to the ability to run code or read protected files with higher privileges.

## 6. Verify root access and document the path

If the lab's intended escalation succeeds, verify the current identity with whoami and id before accessing the root flag. Record the actual weakness and the steps that prove it. Do not claim root access from a command that merely ran without an error.

The report should explain the initial exposure, the path to user access, the privilege escalation weakness, and how the system owner could fix each issue. Typical mitigations include removing sensitive files from web roots, rotating exposed credentials, restricting sudo permissions, and correcting unsafe SUID or service configurations.

## Troubleshooting

- No open ports: confirm the machine is running and that you copied its current IP.
- Web page does not load: verify the HTTP port and whether the lab has finished booting.
- A discovered file is inaccessible: check its actual response and the path, rather than assuming it is a credential.
- SSH fails: verify the username and authentication method from the evidence.
- No escalation path is obvious: re-check permissions and the exact command output before trying more tools.

## What I took from this room

GamingServer rewards careful enumeration more than random exploitation. I move from exposed services to evidence, verify each credential or access path, and only then investigate privilege escalation. The flags and machine-specific results must come from the active target.