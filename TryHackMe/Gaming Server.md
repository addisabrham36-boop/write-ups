# GamingServer: TryHackMe Walkthrough

Room: https://tryhackme.com/room/gamingserver

GamingServer is a beginner boot-to-root machine. The chain is web enumeration, an exposed SSH private key, cracking its weak passphrase, SSH access as the user, and privilege escalation through unsafe LXD group membership. The flags are omitted here; retrieve them from your own live instance.

## 1. Scan the target

Start the lab and record the assigned IP. Run:

    nmap -sC -sV -oN gamingserver-nmap.txt TARGET_IP

The intended machine exposes SSH and HTTP. Confirm the actual ports and service versions from your own scan. If the initial scan misses something, run a full TCP scan:

    nmap -p- --min-rate 1000 -oN gamingserver-allports.txt TARGET_IP

## 2. Inspect the web page

Open the HTTP service and inspect the homepage source. The page contains a username clue in its HTML comments. This is a reminder that comments hidden from normal page rendering are still sent to the browser.

Run directory enumeration against the target:

    gobuster dir -u http://TARGET_IP/ -w /path/to/wordlist.txt

Use a wordlist available in the AttackBox. The discovered /secret path contains an SSH private key, and another exposed path provides a dictionary file. Verify the actual paths and filenames in the active lab before downloading them.

For example, once the paths have been confirmed:

    curl -f http://TARGET_IP/secret/secretKey -o secretKey
    chmod 600 secretKey

Do not use a fixed IP or copy files from another instance. These commands are examples of retrieving the files from your own authorised target.

## 3. Crack the SSH key passphrase

The private key is protected by a passphrase. John the Ripper can test candidate passphrases against a hash extracted from the key. Use the SSH-to-John conversion script installed with your lab tools:

    ssh2john secretKey > secretKey.hash
    john --wordlist=/path/to/dict.list secretKey.hash

The dictionary path should point to the file discovered in the room. The passphrase is weak enough to be found from that supplied list. Verify the candidate through the intended SSH login rather than assuming any line printed by a cracking tool is correct.

Protect private-key files with restrictive permissions and never place them in a public web directory. A passphrase helps, but a weak passphrase can still be guessed offline.

## 4. Connect over SSH

Use the username identified from the page source and the recovered key:

    ssh -i secretKey USERNAME@TARGET_IP

Replace USERNAME with the value found in the room. If SSH rejects the key, check that you are using the correct user and key, that the key file permissions are restricted, and that the passphrase is correct.

After login, verify the account and inspect the home directory:

    whoami
    id
    pwd
    ls -la

Locate the user flag as directed by the room and submit it in TryHackMe. The flag is intentionally not reproduced in this public write-up.

## 5. Identify the LXD privilege-escalation path

The user belongs to the lxd group. LXD manages system containers, and membership can be highly privileged because a user may be able to create a privileged container and mount the host filesystem into it. This is not a normal low-privilege permission; group membership should be restricted to trusted administrators.

Confirm the group membership with id. Then follow the room's LXD escalation method using an Alpine image built for the lab's architecture. If the image is built on another system, ensure it is compatible with the target.

The general sequence is to import the image, initialise a privileged container, attach the host root filesystem as a disk device, start the container, and open a shell inside it. The commands below show the shape of the workflow; use the actual image filename and alias you imported:

    lxc image import IMAGE_FILE --alias labimage
    lxc init labimage labcontainer -c security.privileged=true
    lxc config device add labcontainer hostroot disk source=/ path=/mnt/root recursive=true
    lxc start labcontainer
    lxc exec labcontainer /bin/sh

Inside the container, inspect the mounted host root directory. Verify the filesystem path before reading the root flag. Do not run this technique on a host outside the lab.

## 6. Verify root access and document the fix

The mounted filesystem provides access to the host's root files because the container was created with privileged access and a host-root mount. Verify the target paths and retrieve the root flag from the live machine. Do not publish the flag value.

The defensive fixes are straightforward: remove unnecessary public access to the SSH private key, rotate the key, enforce a strong passphrase, remove the user from the lxd group if they do not need it, and restrict container management privileges.

## What I took from this room

GamingServer chains two avoidable misconfigurations. A private key should never be exposed by the web server, and LXD administration should not be granted casually. Enumeration revealed both weaknesses, while verification at each stage prevented me from guessing the final result.

## Phase 1: Refine enumeration

Preserve the initial scan and use it to direct the next action:

```bash
nmap -sC -sV -oN gamingserver-nmap.txt TARGET_IP
nmap -p- -T4 -oN gamingserver-allports.txt TARGET_IP
```

The first command runs default scripts and attempts service/version detection. The second enumerates all TCP ports with a faster timing profile. Run the full scan only inside the authorized room and inspect the output for any service missed by the initial scan.

The web page includes a username clue in an HTML comment. View the source in the browser or retrieve it for inspection:

```bash
curl -i http://TARGET_IP/
```

Use Gobuster against the HTTP service and follow the hidden-directory clue in the page source. Record the exact path and verify the server returns the private-key file and dictionary file described in the room.

## Phase 2: Recover the SSH key passphrase

Download the private key from the discovered path into the local lab workspace. Keep the file private and restrict its permissions:

```bash
curl -f http://TARGET_IP/secret/secretKey -o secretKey
chmod 600 secretKey
file secretKey
```

Convert the key into a format suitable for John the Ripper:

```bash
ssh2john secretKey > secretKey.hash
john --wordlist=./dict.lst secretKey.hash
john --show secretKey.hash
```

ssh2john extracts a passphrase-checking representation; John tests candidate words from the room's dictionary. The show option displays any recovered result. Use the actual dictionary filename found on the machine.

The passphrase is weak enough to appear in the provided list. Protect private keys with a strong unique passphrase and never store them in publicly served directories.

## Phase 3: Initial access and LXD privilege escalation

Connect using the username obtained from the comment and the recovered key:

```bash
ssh -i secretKey USERNAME@TARGET_IP
whoami
id
pwd
ls -la
```

Confirm the effective account and groups. The intended escalation clue is membership in the lxd group. LXD manages system containers, and membership can be highly privileged because a user may be able to create a privileged container and access host resources depending on the version and configuration.

Use the exact container procedure specified by the current room. Before running it, establish whether an image is available, whether LXD is configured, and whether the current user can create and configure containers. The resulting access path depends on the room's host configuration.

If the intended technique succeeds, verify the resulting host context with id, hostname, and the path from which you read the root flag. Do not treat root inside a container as host root unless the host filesystem access demonstrates it.

## Phase 4: Flags and task questions

The user flag is located in the user home directory as directed by the room. The root flag becomes readable only after the LXD misconfiguration has been successfully abused. Read both from the live machine; flag values are not assumed to be the same across instances.

## Phase 5: Remediation

- Never expose private SSH keys through the web root.
- Protect keys with strong passphrases and remove them from public storage.
- Restrict LXD group membership to trusted administrators.
- Use unprivileged containers and restricted profiles; do not expose host devices or filesystem paths unnecessarily.
- Keep container management software and the host OS updated.
- Audit user/group membership and alert on unexpected container creation.
- Treat container managers as privileged control planes, not ordinary user tools.
