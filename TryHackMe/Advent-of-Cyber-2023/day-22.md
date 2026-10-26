# Advent of Cyber 2023 - Day 22: SSRF

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Server-Side Request Forgery (SSRF) exploitation to access internal metadata services and rogue C2 administration panels.

## Walkthrough
1. SSRF allows attackers to induce the server to make requests to internal resources as well as external destinations (answering `nay` to external-only).
2. Exploit a URL import parameter to target internal port `8000`:
   ```text
   http://127.0.0.1:8000/api/status
   ```
3. C2 panel reveals version `1.1` and default credentials (`admin`).
4. Logging into the panel gives: `THM{SSRF_L0OTED_SUCC3SS}`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Is SSRF restricted to external resources only? (yea/nay) | `nay` |
| What is the C2 version? | `1.1` |
| Username for accessing the C2 panel? | `admin` |
| Flag after accessing C2 panel? | `THM{SSRF_L0OTED_SUCC3SS}` |
