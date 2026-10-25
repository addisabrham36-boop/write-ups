# GamingServer: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/gamingserver

## Room Overview & Information

**Category:** Linux boot-to-root / privilege escalation  
**Difficulty:** Beginner  
**Attack chain:** Web reconnaissance → exposed SSH private key → offline passphrase recovery → SSH access → abuse of LXD group membership  
**Target:** The active TryHackMe machine only.

The target address, host configuration, and flag values may vary between deployments. Replace TARGET_IP with the address assigned to your current lab instance. This guide explains how to verify each stage; it does not invent machine-specific flag values.

## Phase 1: Reconnaissance & Enumeration

### 1. Scan the target

Start the machine and record its current IP. Run a service/version scan and save the output:

```bash
nmap -sC -sV -oN gamingserver-nmap.txt TARGET_IP
```

- `-sC` runs Nmap's default scripts.
- `-sV` attempts to identify service versions.
- `-oN` saves human-readable results.
- `TARGET_IP` is the current lab address.

The intended machine exposes SSH and HTTP. If those results do not explain the service surface, and the room permits it, run a full TCP scan:

```bash
nmap -p- -T4 -oN gamingserver-allports.txt TARGET_IP
```

`-p-` scans TCP ports 1 through 65535. `-T4` uses a faster timing profile, which is generally suitable for many labs but may be less reliable on slow links. Only enumerate the assigned host.

### 2. Inspect the website and page source

Open the HTTP service in the browser. Inspect the homepage source with the browser's View Page Source command or retrieve it:

```bash
curl -i http://TARGET_IP/
```

The `-i` option displays response headers as well as the body. Search the HTML comments for clues about the user or the location of a private key. HTML comments are delivered to the browser and can reveal information even when they are not visible on the rendered page.

### 3. Discover the exposed files

Enumerate paths with Gobuster using a wordlist present in the AttackBox:

```bash
gobuster dir -u http://TARGET_IP/ -w /usr/share/wordlists/dirb/common.txt -o gamingserver-web.txt
```

- `dir` performs directory/file discovery.
- `-u` sets the base URL.
- `-w` selects the wordlist.
- `-o` saves results.

Inspect the discovered paths manually. The intended trail leads to the exposed `/secret/` location containing an SSH private key, and a separate path containing the dictionary file used later. A discovered path is only a lead; open it and verify its contents in the active instance.

Download the key into the local lab workspace after confirming its exact path:

```bash
curl -f http://TARGET_IP/secret/secretKey -o secretKey
chmod 600 secretKey
file secretKey
sha256sum secretKey
```

The `-f` option makes curl fail on an HTTP error status, `-o` specifies the local output filename, and `chmod 600` restricts file access to the owner. The hash records which key copy was analysed. Do not publish the private key.

## Phase 2: Vulnerability Analysis & Initial Access

### 4. Recover the SSH key's passphrase

A private key protected by a passphrase can be tested offline. The room provides a dictionary file; use that file rather than an unrelated wordlist.

First locate the dictionary file from the web enumeration results and save it in the working directory. Convert the encrypted key to John's format:

```bash
ssh2john secretKey > secretKey.hash
john --wordlist=./dict.lst secretKey.hash
john --show secretKey.hash
```

- `ssh2john` creates a representation of the key that John the Ripper can test.
- `--wordlist` tells John which candidate list to use.
- `--show` displays a previously recovered result.

Replace `dict.lst` with the exact dictionary filename found in your instance. The path may differ from the example. If no candidate is recovered, confirm the key file is complete, that the conversion succeeded, and that you selected the correct dictionary. Do not assume that a cracking tool succeeded unless it reports a result.

The passphrase belongs only to the deliberately vulnerable lab key. Use a strong, unique passphrase for real private keys.

### 5. Connect over SSH and verify the user

Use the username revealed by the page source and the recovered key:

```bash
ssh -i secretKey USERNAME@TARGET_IP
```

Replace USERNAME with the exact user identified in the room. If SSH requests a passphrase, enter the recovered lab passphrase. If the server rejects the key, check the username, target IP, key file, and local key permissions.

After login, capture the current context:

```bash
whoami
id
hostname
pwd
ls -la
```

These commands verify the account, group memberships, hostname, location, and home-directory contents. Retrieve the user flag from the location indicated by the room and submit the value shown by the active machine. Keep the flag redacted in public notes as `flag{hidden}`.

## Phase 3: Privilege Escalation & Post-Exploitation

### 6. Identify the LXD group membership

Check the current user's groups:

```bash
id
getent group lxd
```

The intended escalation clue is membership in the `lxd` group. LXD manages system containers. Depending on configuration, a user with container-management rights may be able to create a privileged container and mount host filesystem paths into it. This is a powerful administrative capability, not an ordinary user permission.

### 7. Understand and validate the container path

The room's technique imports a compatible Alpine image, creates a privileged container, mounts the host root filesystem into the container, and enters a shell. The image must match the target architecture and must be available to the lab user.

The workflow should be verified stage by stage:
1. Confirm LXD is installed and the current account can manage it.
2. Import the room's image and confirm it appears in the LXD image list.
3. Create the container with the privileged setting shown in the official task.
4. Attach the host root filesystem as a disk device at the specified mount path.
5. Start the container and verify that it is running.
6. Enter the container and inspect the mounted path.
7. Confirm that the files belong to the host filesystem, not only to the container.
8. Retrieve the root flag only after the host-level access is demonstrated.

Do not run the procedure against a production host. A root shell inside a normal unprivileged container does not automatically equal root on the host. In this room, the dangerous combination is container-management permission plus a privileged container and a writable host-root mount.

### 8. Verify root access and retrieve the flag

After the host filesystem is mounted, inspect the documented mount path and verify the expected target file before reading it. Record the exact path and permissions that permit access. Copy the flag from the active machine; do not replace it with a value from an online walkthrough.

## Phase 4: Room Questions & Evidence Checklist

| Objective | Evidence required |
|---|---|
| Identify the exposed services | Saved Nmap report from the current target |
| Find the username clue | HTML source comment on the homepage |
| Locate the private key and wordlist | Gobuster findings and manual path validation |
| Recover the passphrase | John output tied to the downloaded key and room dictionary |
| Obtain initial access | Successful SSH login plus whoami/id output |
| Identify the escalation path | LXD group membership and permitted container-management actions |
| Obtain root access | Verified host filesystem access and root-level context |
| Retrieve user/root flags | Exact flag files from the active instance, redacted publicly |

The room-specific username, passphrase, and flag values must be read from the active target if the current instance differs. A successful login or a container shell alone is not sufficient evidence of root-level host access.

## Phase 5: Key Takeaways & Remediation

- Never expose SSH private keys or wordlists through a public web root.
- Rotate any private key that was publicly exposed and remove the served copy from the deployment.
- Protect keys using restrictive permissions and strong passphrases.
- Restrict LXD group membership to trusted administrators.
- Avoid granting ordinary users the ability to create privileged containers or mount host paths.
- Use unprivileged containers, restricted profiles, and least-privilege device and filesystem access.
- Audit container-management activity and alert on unexpected container creation.
- Patch and monitor the host operating system and container runtime.

GamingServer combines information disclosure with an overly privileged container-management group. Each transition should be justified by a concrete result: scan, discovered file, recovered key passphrase, verified SSH identity, and finally a proven host privilege boundary failure.