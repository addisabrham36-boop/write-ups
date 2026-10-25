# Enumeration & Brute Force: TryHackMe Walkthrough

Room: https://tryhackme.com/room/enumerationbruteforce

This room covers authentication enumeration and controlled password testing. The core lesson is that authentication errors and response differences can leak information, while weak passwords and missing rate limits can allow repeated guessing. Use only the machine assigned by the room.

## 1. Connect to the target

Start the lab and record the machine's current IP. Add the hostname mapping required by the room to /etc/hosts using the supplied IP and hostname. Verify that the name resolves before opening the web application.

If the room recommends the AttackBox, use it or connect your own testing machine to the authorised TryHackMe VPN. Do not substitute an unrelated host.

## 2. Inspect the login workflow

Open the login page and observe the normal request. Identify the request method, path, field names, cookies, and response behaviour. Submit a known invalid username and password, then compare the response with another controlled test.

Look for differences such as:
- Different error text for unknown users and incorrect passwords.
- Different response codes or redirects.
- Different response lengths or timing.
- Whether account lockout or rate limiting is triggered.

These differences can enable username enumeration. Timing differences need repeated controlled measurements and should not be treated as proof after one request.

## 3. Use Burp Suite in the lab

If the room uses Burp Suite, route the browser through the configured proxy and inspect the request in the Proxy history. Send a copy to Repeater to change one parameter at a time. Compare the responses while keeping the rest of the request constant.

When using Intruder or another automated feature, restrict the test to the room's target and the candidate list permitted by the task. High request rates can trigger lockouts or make the lab unstable.

## 4. Understand brute-force testing

Brute-force testing tries a set of candidate credentials against an authentication mechanism. The method is only useful if the username, endpoint, request format, and failure condition are correct. A response that looks different is a candidate result that must be verified.

The room may provide a particular username list, password list, or form field. Use those exact files and settings. Do not assume a common wordlist or a guessed credential will match the current lab.

## 5. Validate the result

When a candidate appears to work, verify it through the intended login interface and confirm the account or page reached. Record the evidence and stop once the task's objective is met. Do not explore unrelated user data.

If no candidate succeeds, re-check the request fields, cookies, failure message, wordlist, and whether the account exists. Do not respond by expanding the attack beyond the room's scope.

## 6. Defences

Applications should use generic authentication errors, rate limiting, monitoring, MFA, strong password policy, and secure session management. Rate limits should be designed carefully to avoid allowing an attacker to lock out other users. Login flows should avoid leaking whether an account exists through content or timing differences.

## 7. Working through the room

Follow each task in order. First identify how the application reveals authentication behaviour, then use the provided candidate data to test the specific weakness. Copy values from the live response when a question requests a concrete result. Do not insert guessed usernames, passwords, or flags into the public note.

## What I took from this room

Authentication testing is more than sending many passwords. The analyst needs to understand the request, recognise information leaks, keep testing controlled, and verify a candidate result. Generic errors, rate limits, MFA, and monitoring make these attacks harder to carry out.