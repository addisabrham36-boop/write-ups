# Advent of Cyber 2022 - Day 4: Scanning

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Network reconnaissance using Nmap to discover active network services, identify operating system versions, and enumerate SMB (Server Message Block) file shares.

## Walkthrough
Perform a comprehensive TCP port scan against the target QA server:
```bash
nmap -sC -sV -p- -T4 TARGET_IP
```
Scan results:
- Port 22/tcp: `OpenSSH 8.2p1` (`ssh`)
- Port 80/tcp: `Apache httpd 2.4.41` (`Apache`)
- Port 139/445/tcp: `Samba smbd 4.6.2`

Enumerating SMB shares:
```bash
smbclient -L //TARGET_IP/ -N
smbclient //TARGET_IP/santa_anon -N
```
Accessing the open share yields a hidden text file containing the Samba flag: `THM_SANTA_SMB_SERVER`. Furthermore, inspecting configuration notes discloses the password for account `santahr` as `santa25`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the name of the HTTP server running on the remote host? | `Apache` |
| What is the name of the service running on port 22 on the QA server? | `ssh` |
| What flag can you find after successfully accessing the Samba service? | `THM_SANTA_SMB_SERVER` |
| What is the password for the username santahr? | `santa25` |
