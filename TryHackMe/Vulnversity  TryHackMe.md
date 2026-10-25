# Vulnversity: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/vulnversity

## Room Overview & Information

**Platform:** TryHackMe  
**Difficulty:** Easy  
**Category:** Linux penetration testing, web application testing, privilege escalation  
**Learning objectives:** Nmap service discovery, Gobuster directory enumeration, Burp Suite upload testing, controlled reverse-shell validation, SUID permission analysis, and remediation.

This is a deliberately vulnerable training machine. Perform the steps only against the instance deployed for the room. The machine IP changes between deployments, so every command below uses `MACHINE_IP` as a placeholder. Replace it with the current address shown by TryHackMe. Do not copy a historical target IP from a screenshot.

## Phase 1: Reconnaissance & Enumeration

### Task 1: Deploy the machine

Start the TryHackMe machine and use the AttackBox or your authorized VPN-connected environment. Wait until the platform reports that the machine is ready, then record the assigned IP.

### Task 2: Reconnaissance with Nmap

Start with service/version detection:

```bash
nmap -sV -oN vulnversity-initial.txt MACHINE_IP
```

- `-sV` probes open ports to identify the service and likely version.
- `-oN` saves human-readable output in the named file.
- `MACHINE_IP` is the currently deployed target.

If time permits and the room allows it, use a broader scan to ensure high-numbered TCP ports are not missed:

```bash
nmap -sC -sV -p- -T4 -oN vulnversity-full.txt MACHINE_IP
```

- `-sC` runs Nmap's default scripts.
- `-sV` attempts service/version identification.
- `-p-` scans all TCP ports from 1 through 65535.
- `-T4` uses a faster timing template, appropriate for many lab networks but not a guarantee of reliability on slow or unstable links.
- `-oN` saves the result for later reference.

A representative room scan identifies six open TCP ports: 21 (FTP), 22 (SSH), 139 and 445 (SMB), 3128 (Squid proxy), and 3333 (Apache HTTP). Your instance's output is authoritative.

### Question: “Scan the box; how many ports are open?”

**Answer:** `6`.

Count the open ports in the actual Nmap result. Do not count closed or filtered ports.

### Question: “What version of the squid proxy is running on the machine?”

**Answer:** `3.5.12`.

In the Nmap output, find the service on TCP 3128. The service/version line identifies Squid HTTP proxy 3.5.12 in the standard room instance. Service banners can be inaccurate in other environments, so verify your result.

### Question: “How many ports will Nmap scan if the flag -p-400 was used?”

**Answer:** `400`.

The syntax `-p-400` means TCP ports 1 through 400. This is different from `-p-`, which scans the entire port range.

### Question: “Using the nmap flag -n what will it not resolve?”

**Answer:** `DNS`.

The `-n` option disables reverse DNS resolution. This can speed scans and avoid extra name-lookup traffic.

### Question: “What is the most likely operating system this machine is running?”

**Answer:** `Ubuntu`.

Use service fingerprints and the information reported by Nmap. OS detection can be uncertain, especially through filters or virtual networking, so phrase this as the most likely OS rather than absolute proof.

### Question: “What port is the web server running on?”

**Answer:** `3333`.

The HTTP service is exposed on TCP port 3333, rather than the default port 80. Always use the port found during enumeration.

### Question: “What is the flag for enabling verbose mode using Nmap?”

**Answer:** `-v`.

Lowercase `-v` increases verbosity. `-vv` increases it further. Uppercase `-V` is not the same option.

### Task 3: Locate directories with Gobuster

The web server is on port 3333. Enumerate directories and files using a wordlist available in the lab:

```bash
gobuster dir -u http://MACHINE_IP:3333/ -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt -o gobuster.txt
```

- `dir` selects directory/file enumeration mode.
- `-u` supplies the base URL.
- `-w` supplies the wordlist path.
- `-o` saves the results.

The wordlist path differs between distributions. Check that the file exists before starting. If your scan shows many responses with the same status and size, investigate whether the application returns a custom “not found” page and tune the scan accordingly.

The important discovery in this room is the `/internal/` path, which contains the upload form. A found directory is a lead: open it in the browser and inspect its actual functionality before drawing a conclusion.

### Question: “What is the directory that has an upload form page?”

**Answer:** `/internal/`.

Gobuster identifies the path, then manual browsing confirms that it contains the file-upload form.

## Phase 2: Vulnerability Analysis & Initial Access

### Task 4: Test the upload filter

An upload form that accepts arbitrary server-executable files can become a code-execution path. Here, the exercise demonstrates that the application blocks a common PHP extension but accepts an alternative extension that the web server still executes.

Use Burp Suite only within this lab. Intercept a normal upload, send the request to Intruder, and mark only the filename extension as the payload position. A short extension list is enough for the intended exercise:

```text
.php
.php3
.php4
.php5
.phtml
```

Use a Sniper attack to test one marked position per request. Compare status codes, response lengths, and error messages, then verify the accepted file type through the lab's normal upload workflow. A different response size is a clue, not conclusive proof; manually confirm the result.

### Question: “What common file type you'd want to upload to exploit the server is blocked? Try a couple to find out.”

**Answer:** `.php`.

The upload validation rejects the conventional PHP extension.

### Question: “What extension is allowed after running the above exercise?”

**Answer:** `.phtml`.

In the intended room configuration, the test identifies `.phtml` as an accepted extension. Do not assume that extension is accepted by other servers.

### Task 4: Validate code execution

The room uses a PHP reverse shell to demonstrate the impact of an unsafe upload. A reverse shell causes the target to initiate a connection back to the tester. Use an address reachable from the lab target, normally the VPN/AttackBox interface, and a port allowed by the exercise.

Start a listener:

```bash
nc -lvnp 4444
```

- `-l` listens.
- `-v` prints connection details.
- `-n` disables name resolution.
- `-p 4444` selects the listening port.

Configure the supplied PHP reverse-shell file to use the attack machine's lab-reachable IP and port 4444, save it with the accepted `.phtml` extension, upload it, and request the uploaded path shown by the room. Do not use a public IP or a host outside scope.

When a connection arrives, verify the identity and host rather than assuming the shell is privileged:

```bash
whoami
id
pwd
hostname
```

### Question: “What is the name of the user who manages the webserver?”

**Answer:** `bill`.

Confirm this from the live shell or the room's provided evidence. A web-server process can run as a service account while the application or user files are owned by a named local user; answer the question according to the task's wording and observed account.

### Question: “What is the user flag?”

**Answer from the recorded room instance:** `8bd7992fbe8a6ad22a63361004cfcedb`.

The flag is read from the user-owned location indicated by the room after establishing the intended foothold. If your deployed instance differs, submit the value from your own machine.

## Phase 3: Privilege Escalation & Post-Exploitation

### Task 5: Enumerate SUID binaries

SUID causes a program to run with the effective UID of its file owner when executed. It is sometimes needed for legitimate system functions, but an unusual SUID program can expose a privilege escalation path.

List SUID files:

```bash
find / -perm -4000 -type f 2>/dev/null
```

- `/` searches from the filesystem root.
- `-perm -4000` matches files with the SUID permission bit.
- `-type f` restricts results to regular files.
- `2>/dev/null` suppresses expected permission-denied messages.

Compare the output with normal system binaries and investigate unusual entries. In this room, `/bin/systemctl` is the notable SUID binary. Check its owner and permissions before proceeding:

```bash
ls -l /bin/systemctl
```

The significance is not that systemctl is inherently malicious; the dangerous condition is assigning elevated SUID execution to a powerful service-management utility.

Use the room's intended SUID/systemctl technique as a lab-only proof. The exploitation path relies on the privileged binary being able to influence a systemd unit or execute a command with elevated privileges. Do not apply this technique to a production host.

### Question: “On the system, search for all SUID files. Which file stands out?”

**Answer:** `/bin/systemctl`.

### Question: “What is the root flag value?”

**Answer from the recorded room instance:** `a58ff8579f0a9270368d33a9966c7fd5`.

Read the root flag only after verifying elevated access in the lab. A file being visible through an error message or another accidental disclosure is not a substitute for understanding the privilege boundary that made it accessible.

## Phase 4: Room Questions & Flags Walkthrough

| Task | Question focus | Answer or validation method |
|---|---|---|
| Reconnaissance | Open ports | Count actual Nmap results; standard instance: 6 |
| Reconnaissance | Squid version | Read TCP 3128 service output; standard instance: 3.5.12 |
| Reconnaissance | `-p-400` | Ports 1 through 400; answer 400 |
| Reconnaissance | `-n` | DNS resolution disabled |
| Reconnaissance | Likely OS | Ubuntu |
| Reconnaissance | Web port | TCP 3333 |
| Reconnaissance | Verbose flag | Lowercase `-v` |
| Directory enumeration | Upload directory | `/internal/` |
| Upload test | Blocked extension | `.php` |
| Upload test | Allowed extension | `.phtml` |
| Foothold | Web-server manager/user | `bill` |
| Foothold | User flag | `8bd7992fbe8a6ad22a63361004cfcedb` in the recorded instance |
| Privilege escalation | Notable SUID file | `/bin/systemctl` |
| Privilege escalation | Root flag | `a58ff8579f0a9270368d33a9966c7fd5` in the recorded instance |

## Phase 5: Key Takeaways & Remediation

1. **Enumerate all ports.** Non-standard ports often expose important services.
2. **Do not trust extension-only upload filters.** Validate file content, store uploads outside executable web paths, assign server-generated names, and disable script execution in upload directories.
3. **Do not run web services with unnecessary privileges.** Separate application users from administrative accounts.
4. **Audit SUID permissions.** Remove SUID from utilities that do not require it, and review changes through configuration management.
5. **Restrict service-management permissions.** Only trusted administrators should be able to create or start privileged systemd units.
6. **Patch and monitor.** Keep the operating system and web components updated, log uploads and service changes, and alert on unexpected privileged process execution.

The room demonstrates a complete chain: discovery leads to an upload weakness, the upload weakness leads to an initial shell, and a dangerous SUID configuration provides privilege escalation. Each link should be backed by the current lab's evidence.