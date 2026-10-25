# RA: Oh My WebServer — TryHackMe Walkthrough

Room: https://tryhackme.com/room/ohmywebserver

This room is a vulnerable-server challenge. The reliable approach is to enumerate the exposed services, identify the web server and its version, validate the relevant weakness, and verify impact on the assigned machine. The target IP and service configuration can change between instances.

## 1. Start the lab and scan the target

Start the machine and record its current address. Run a basic service scan:

    nmap -sC -sV -oN ohmywebserver-nmap.txt TARGET_IP

If needed, scan all TCP ports:

    nmap -p- --min-rate 1000 -oN ohmywebserver-allports.txt TARGET_IP

Use the address supplied by the room. Review open ports and service banners. A server banner can help identify the software, but it is not sufficient by itself to prove that a vulnerability is present.

## 2. Inspect the web service

Open the web server in a browser and record the status code, page content, headers, and redirects. Use the correct scheme and port from the scan. If the default page is missing or an error appears, check whether the service expects a particular hostname or path.

If the room asks you to enumerate directories, use a wordlist available in the AttackBox and review promising paths manually. Do not assume a directory exists because it appeared in an unrelated scan.

## 3. Identify the server and relevant vulnerability

Compare the detected software and version with the room's clue and reliable documentation. Check whether the vulnerability applies to that exact version and configuration. Vendors may backport fixes, and proxies may alter the visible banner.

If the task demonstrates a server-side weakness, identify the affected endpoint or request and reproduce the smallest safe proof. Avoid destructive tests or actions that could affect other users.

## 4. Validate access and impact

If the intended exercise grants access to the host, verify the current user and host identity before proceeding. A web-server process may run under a restricted account, so successful code execution is not automatically root access.

Record the exact evidence that proves the issue: the request, response, server behaviour, or session details. Do not claim a successful exploit merely because a tool printed a completion message.

## 5. Remediation

Keep the web server and dependencies patched, remove unused modules, disable unnecessary features, and restrict the service account's permissions. Hide detailed error messages from public responses while retaining appropriate server-side logs. Monitor unusual requests and unexpected child processes.

## 6. Working through the room

Follow the current task's hints and use the target's actual output to select the next step. If the task asks for a version, service, or file, copy it from the lab. If an exploit fails, verify the version, port, and prerequisites before trying another approach.

## What I took from this room

A vulnerable web server should be approached like any other target: discover the service, confirm the relevant weakness, demonstrate impact safely, and recommend a fix. The exact machine details must come from the active lab, not a remembered output.