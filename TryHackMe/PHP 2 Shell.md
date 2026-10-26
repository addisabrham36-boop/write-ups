# PHP 2 Shell

{% embed url="https://tryhackme.com/room/cvephp2shell" %}

This room covers a critical two-CVE chain in PHP's runtime: **CVE-2026-109618** (memory disclosure / ASLR bypass via `session.upload_progress`) and **CVE-2026-109619** (use-after-free leading to RCE). It's a guided room — you follow along with an SSH session while reading the theory.

---

## Setup

SSH into the target with the provided credentials:

```bash
ssh ubuntu@MACHINE_IP
```

---

## The Vulnerability Chain

### CVE-2026-109618 — Memory Leak

PHP's `session.upload_progress` feature is enabled by default and processes multipart form data **before** any application code runs. When an out-of-memory (OOM) condition is forced during upload tracking, PHP skips its cleanup routines and leaves stale pointer data in memory.

Crafted requests targeting the upload progress endpoint leak process memory — enough to defeat ASLR (Address Space Layout Randomization).

### CVE-2026-109619 — Use-After-Free → RCE

With ASLR bypassed, the use-after-free condition from the stale global state can be exploited to hijack control flow. Combined with the leaked addresses from CVE-2026-109618, we get reliable RCE as the web server user.

---

## Exploitation

Run the exploit chain from the lab environment:

```bash
# The room provides the exploit script — run it against localhost
python3 exploit.py http://MACHINE_IP/
```

Once RCE is confirmed, read the flag:

```bash
cat /flag.txt
# or wherever the room places it
```

---

## Detection & Mitigation

The only complete fix is updating PHP to patched versions:
- PHP **8.5.12**, **8.4.27**, **8.3.36**, or **8.2.35**

Disabling `session.upload_progress` in `php.ini` is a partial workaround but doesn't fully close the attack surface.

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| CVE for memory disclosure / ASLR bypass | `CVE-2026-109618` |
| CVE for RCE via use-after-free | `CVE-2026-109619` |
| What PHP feature is abused? | `session.upload_progress` |
| Minimum patched PHP versions | `8.5.12, 8.4.27, 8.3.36, 8.2.35` |
| Flag | Retrieved from the target after running the exploit (format `THM{...}`) |
