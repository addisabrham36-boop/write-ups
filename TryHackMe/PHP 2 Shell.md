# PHP 2 Shell: TryHackMe Walkthrough

Room: https://tryhackme.com/room/php2shell

This room concerns the path from a PHP web application weakness to server-side command execution. The exact target behaviour must be verified in the active lab. The important skill is to understand how untrusted input reaches a PHP interpreter or operating-system command and how to demonstrate the impact without affecting anything outside the authorised machine.

## 1. Confirm the target and enumerate

Start the lab and record the assigned IP. Identify the exposed services:

    nmap -sC -sV -oN php2shell-nmap.txt TARGET_IP

If necessary, scan all TCP ports:

    nmap -p- --min-rate 1000 -oN php2shell-allports.txt TARGET_IP

Open the web service and inspect the application, parameters, forms, and responses. Save notes about the endpoints that accept input. A parameter is not vulnerable merely because it accepts text; the key question is how the server uses that input.

## 2. Trace the input path

Look for functionality that processes files, runs conversions, performs lookups, or invokes a server-side utility. In PHP, dangerous patterns can arise when user-controlled data is passed to shell execution functions or concatenated into queries and commands.

In the lab, identify the exact parameter or feature highlighted by the task. Compare normal input with a harmless test value and observe whether the response changes. Use the smallest proof that confirms the issue. Avoid destructive commands and never test on a real website without written authorisation.

## 3. Understand the difference between PHP and a shell

PHP code executes within the web application runtime. A shell is an operating-system command interpreter. A vulnerability may allow input to cross from the application into a shell, but the path depends on the function and how arguments are constructed.

If the application passes a string to a shell, characters that have special meaning to that shell may change how the command is parsed. Safer code avoids shell invocation when possible, uses APIs that accept structured arguments, validates input against a strict allowlist, and never treats user input as trusted command syntax.

## 4. Verify impact safely

A useful proof in a lab demonstrates a harmless result, such as identifying the execution context or returning a fixed benign value. Record the request, response, and the evidence that server-side execution occurred. Do not use commands that delete files, alter accounts, or access unrelated secrets.

If a shell is obtained as part of the authorised task, verify the identity and host before any further action. The permissions of the web-server account usually determine what the process can access; command execution does not automatically mean root access.

## 5. Investigate the application context

After confirming the weakness, determine which application code path is responsible, what user input reaches it, and which operating-system identity executes it. Inspect relevant logs and configuration only within the room's scope. Do not copy real credentials or private configuration into a public note.

## 6. Remediation

- Avoid shell commands for tasks that can be performed with safe library APIs.
- If a subprocess is necessary, pass a fixed executable and structured arguments rather than concatenating strings.
- Validate input with an allowlist appropriate to the expected data.
- Run the web application with minimal permissions.
- Keep PHP and server software patched.
- Disable unnecessary functions only as a supplementary measure, not as the only defence.
- Log and monitor suspicious input and unexpected child processes.

## 7. Working through the room

Follow the task's exact endpoint and input. Identify the vulnerable data flow, test the expected condition, and verify the response. Do not copy a payload or result from a different version of the room without confirming it still applies.

## What I took from this room

Server-side command execution usually comes from a broken trust boundary between user input and a privileged interpreter. The best fix is to remove that unsafe boundary, validate inputs, and reduce the permissions of the process that handles requests.