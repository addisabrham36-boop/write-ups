# Hydra: TryHackMe Walkthrough

Room: https://tryhackme.com/room/hydra

Hydra is a login-testing tool that can try supplied username and password candidates against supported services. In a lab, it helps demonstrate why weak passwords and exposed authentication services are risky. Use it only against the target assigned by the room or another system for which you have explicit permission.

## 1. Enumerate before testing credentials

First identify which services are available and which authentication mechanism the task asks you to test. If the room provides a target IP, use it rather than guessing an address.

A common initial scan pattern is:

    nmap -sC -sV -oN hydra-scan.txt TARGET_IP

This requests default scripts and service-version detection and saves the result to a file. Replace TARGET_IP with the current lab target. Read the output to identify relevant ports, such as SSH or FTP. Do not run a password attack against every open service without understanding the task.

## 2. Prepare the username and password candidates

Hydra needs the correct service, target, and candidate values. A wordlist is a file containing candidate passwords, one per line. The lab may supply a particular list or credential hint. Use the path the environment actually provides.

Check that the file exists and is readable:

    ls -l /path/to/wordlist.txt
    head /path/to/wordlist.txt

The example path is a placeholder. Do not assume a wordlist is installed in the same location on every Linux distribution.

## 3. SSH and FTP examples

For a room that explicitly asks for an SSH login test, the general syntax is:

    hydra -l USERNAME -P /path/to/wordlist.txt ssh://TARGET_IP

For an FTP login test:

    hydra -l USERNAME -P /path/to/wordlist.txt ftp://TARGET_IP

Replace USERNAME, the wordlist path, and target with the values provided by the lab. The lowercase -l option supplies one username; uppercase -P supplies a file of password candidates. The service name tells Hydra which protocol module to use.

If the task provides a list of usernames instead of one known username, Hydra supports a username file with the appropriate option. Do not broaden the attack beyond the specific exercise.

## 4. HTTP form authentication

Web login forms require additional details because Hydra must submit the right fields and recognise a failed login. The room should provide the URL, form parameters, and a failure message or other response condition.

A generic form-based command has this shape:

    hydra -l USERNAME -P /path/to/wordlist.txt TARGET_IP http-post-form "/login:username=^USER^&password=^PASS^:F=Invalid credentials"

This is only a syntax example. The path, field names, and failure text must match the actual lab form. If the failure string is wrong, Hydra may misclassify results. Inspect the form in the browser or authorised proxy and compare a known failed login response before running the test.

## 5. Read the output correctly

Hydra reports attempts and may identify a candidate that appears to authenticate successfully. Treat this as a hypothesis until verified. In the lab, try the reported credential through the intended login interface if the instructions allow it, and confirm that it reaches the expected account or service.

Do not copy credentials from a different machine or assume the same answer persists between lab resets. Machine-specific passwords and target addresses can change.

## 6. Troubleshooting

- **Connection refused:** the service may not be running, the port may be wrong, or a firewall may block access.
- **Timeout:** verify the target address, VPN or AttackBox connection, and service reachability.
- **No matches:** check the username, wordlist path, protocol, and whether the credentials are actually in the list.
- **HTTP results look wrong:** confirm the form path, field names, redirects, and failure condition.
- **Too many requests or lockout:** stop and follow the room's guidance. Real systems may lock accounts or trigger alerts.

Do not attempt to bypass account lockout or evade monitoring on real services. In professional testing, coordinate rate limits and lockout behaviour with the system owner.

## 7. Defensive lessons

Use long unique passwords, multi-factor authentication, rate limiting, account lockout designed to resist denial of service, and monitoring for repeated failed logins. Disable unused services and restrict remote administration to trusted networks. Avoid exposing detailed authentication errors that reveal whether a username exists.

## What I took from this room

Hydra demonstrates that authentication security depends on both password quality and how the service handles repeated attempts. Enumeration should come first, the command must match the actual protocol, and any reported credential must be verified against the authorised lab. I do not invent a successful password or flag when I have not observed it.