# TryHack3M: Bricks Heist — Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/tryhack3mbricksheist

## Room Overview & Information

**Difficulty:** Easy  
**Category:** WordPress exploitation, Linux incident response, malware triage, blockchain OSINT  
**Key concepts:** Nmap, WordPress/Bricks Builder, CVE-2024-25600, remote code execution, process and systemd investigation, miner logs, hexadecimal/Base64 transformations, Bitcoin transaction analysis, and threat attribution.

The room asks the learner to regain access to a compromised WordPress server and investigate a suspected cryptocurrency miner. It is both an exploitation exercise and a defensive investigation. Complete the attack portion only on the TryHackMe machine deployed for this room. The target IP and service state change between deployments.

## Phase 1: Reconnaissance & Enumeration

### 1. Configure the required hostname

Start the target and record its assigned address. The room expects the hostname bricks.thm to resolve to the lab IP. Add a line like the following to the testing machine's hosts file:

```text
MACHINE_IP bricks.thm
```

Replace MACHINE_IP with the address from the current room. On Linux, the file is /etc/hosts. Verify the result:

```bash
getent hosts bricks.thm
```

This checks the local name-resolution path. If the returned IP does not match the active machine, correct the mapping before using the web application.

### 2. Scan the target

Run a service/version scan and save the evidence:

```bash
nmap -sC -sV -oN bricks-nmap.txt bricks.thm
```

- `-sC` runs Nmap's default scripts.
- `-sV` attempts to identify the service and version on open ports.
- `-oN` saves readable output to a file.
- `bricks.thm` uses the hostname the room requires.

If the initial scan does not explain the exposed surface, and the room permits it, scan all TCP ports:

```bash
nmap -p- -T4 -oN bricks-allports.txt bricks.thm
```

Here, `-p-` scans TCP ports 1 through 65535 and `-T4` selects a faster timing template. Review the output rather than assuming a specific set of ports from another session.

### 3. Identify WordPress and Bricks Builder

Open the web service in a browser and inspect the homepage, source, redirects, and common WordPress paths. The room concerns a WordPress site built with Bricks Builder. Use a focused enumeration step to establish what is actually present:

```bash
curl -i http://bricks.thm/
```

The response headers and body help identify redirects, server details, and application content. If WPScan is available in the lab, enumerate the site's visible components:

```bash
wpscan --url http://bricks.thm/ --enumerate p,t
```

This requests plugin and theme enumeration. Tool options and detection output can vary by version, so check the installed tool's help and use the target's actual results. A detected component version is a lead, not proof that the instance is vulnerable.

### 4. Confirm the relevant vulnerability

The room is associated with CVE-2024-25600, an unauthenticated remote code execution issue affecting vulnerable Bricks Builder versions. The vulnerability permits attacker-controlled input to reach PHP evaluation through the affected REST API rendering functionality. Confirm the Bricks Builder version and affected configuration using the evidence in the lab and the official room hints before selecting a proof of concept.

Do not run a proof of concept against a real WordPress site just because it uses Bricks Builder. In a professional assessment, first confirm authorization, version, exposure, and applicable preconditions.

## Phase 2: Vulnerability Analysis & Initial Access

### 5. Validate remote code execution inside the lab

Use the proof-of-concept path specified by the room and keep the target set to bricks.thm. The expected security impact is command execution in the context of the web server. Capture the request or tool invocation and its response so the result is reproducible.

A successful exploit attempt message is not proof by itself. Verify that the intended command ran on the target and determine the execution identity. The room's documented path allows access to the server so that the investigation can continue.

If the task requires a reverse connection, use only an address reachable from the AttackBox or VPN interface assigned to your lab environment, and use a port permitted by the exercise. Do not copy an old callback address from a screenshot. Start the listener in a separate terminal and record whether an incoming connection occurs.

Once access is obtained, collect a short context snapshot:

```bash
whoami
id
hostname
pwd
uname -a
```

These commands identify the current user, group context, host name, working directory, and kernel information. A web-server shell does not automatically imply root privileges.

### 6. Find the hidden text file in the web folder

The room asks for the contents of a hidden text file under the web directory. This is the first specific investigation question. After the authorized shell is available, identify the web root and inspect directory entries rather than guessing the secret:

```bash
pwd
ls -la
find /var/www -maxdepth 5 -type f -name '*.txt' 2>/dev/null
```

- `ls -la` includes dotfiles and displays ownership and permissions.
- `find` searches the relevant web tree for text files.
- `-maxdepth 5` limits how deep the search descends.
- `2>/dev/null` suppresses permission-denied messages.

Read only the file identified by the task. The documented answer is:

```text
THM{fl46_650c844110baced87e1606453b93f22a}
```

This file is a clue to the compromised state of the site. Do not expose other site files that are unrelated to the assessment.

## Phase 3: Incident Investigation & Post-Exploitation

### 7. Identify the suspicious process

The scenario describes a suspected cryptominer. Start by listing processes and inspect their complete command lines:

```bash
ps auxf
pgrep -a -f 'nm-inet-dialog'
```

- `ps auxf` lists processes in a tree-like view, helping show parent/child relationships.
- `pgrep -a -f` searches full command lines and prints them alongside matching process IDs.

The documented process name is `nm-inet-dialog`. Its name resembles a desktop/network component, which can make it less noticeable. Do not conclude maliciousness from the name alone; correlate it with the executable path, parent process, service configuration, miner log, and network behaviour.

### Question: “What is the name of the suspicious process?”

**Answer:** `nm-inet-dialog`.

### 8. Find the systemd service associated with the process

List running services and inspect the unit that appears related to the suspicious process:

```bash
systemctl list-units --type=service --state=running
systemctl status ubuntu.service
systemctl cat ubuntu.service
```

- `list-units` enumerates loaded units.
- `--type=service` filters the results to services.
- `--state=running` filters to active running units.
- `status` shows service state and recent log lines.
- `cat` displays the unit definition, including its execution command and restart settings.

The service associated with the suspicious process in the room is `ubuntu.service`. Inspect the unit for the executable path, user identity, startup configuration, and restart behaviour. A service label alone is not evidence that a service is legitimate; malicious persistence can be named to resemble normal system components.

### Question: “What is the service name affiliated with the suspicious process?”

**Answer:** `ubuntu.service`.

### 9. Locate the miner log file

Investigate the directory associated with the suspicious executable and service. In the documented instance, the relevant files are under /lib/NetworkManager. Confirm this from the unit file and process command line, then list the directory:

```bash
cd /lib/NetworkManager
ls -la
file inet.conf
head -n 30 inet.conf
```

- `cd` changes the working directory.
- `ls -la` lists all entries and their permissions.
- `file` identifies the likely format.
- `head -n 30` reads the first 30 lines, which can be important when a log is appended over time.

The miner log/configuration filename requested by the room is `inet.conf`. Record file ownership, timestamps, and the surrounding content. Do not run or source configuration/log files.

### Question: “What is the log file name of the miner instance?”

**Answer:** `inet.conf`.

### 10. Decode and verify the miner wallet address

The log contains a long encoded value. Copy the exact value from the target and preserve a copy before transforming it. Use CyberChef as described by the room, or reproduce the documented decode process locally. The key technique is to identify the encoding using the string's structure, decode it, then inspect the output for repeated data.

The documented decoded output contains two nearly identical Bitcoin addresses. This is a critical verification step: do not select an address simply because it begins with `bc1`. Compare both candidates character by character and use blockchain transaction evidence to determine which is the actual miner wallet.

The correct address for the room is:

```text
bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qa
```

Do not mistake the second similar-looking candidate, which differs in a few characters, for the answer. Cryptocurrency addresses are strings with meaningful checksums and exact characters; a one-character mismatch can refer to a different or invalid address.

### Question: “What is the wallet address of the miner instance?”

**Answer:** `bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qa`.

### 11. Attribute the wallet using OSINT

The final task asks which threat group is associated with transactions involving the wallet. Use a public blockchain explorer to look up the exact wallet or relevant transaction identified by the room's evidence. Follow the transaction trail and corroborate the wallet connection with a trusted sanctions or threat-intelligence report. Do not assume every wallet touching a transaction is operated by the same actor.

The documented link points to the LockBit ransomware group. Phrase attribution carefully: blockchain activity may support an association through a transaction, but it does not necessarily prove that the entire wallet or server was exclusively controlled by a particular actor.

### Question: “The wallet address used has been involved in transactions between wallets belonging to which threat group?”

**Answer:** `LockBit`.

## Phase 4: Complete Question & Answer Walkthrough

| Exact room question | Answer | Evidence |
|---|---|---|
| What is the content of the hidden .txt file in the web folder? | `THM{fl46_650c844110baced87e1606453b93f22a}` | Hidden text file in the web folder |
| What is the name of the suspicious process? | `nm-inet-dialog` | Process list and command line |
| What is the service name affiliated with the suspicious process? | `ubuntu.service` | systemd unit and service status |
| What is the log file name of the miner instance? | `inet.conf` | File in the NetworkManager-related directory |
| What is the wallet address of the miner instance? | `bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qa` | Decode the logged value and validate the correct candidate |
| The wallet address used has been involved in transactions between wallets belonging to which threat group? | `LockBit` | Public blockchain data plus threat-intelligence/sanctions reporting |

## Phase 5: Key Takeaways & Remediation

### Web application

- Patch Bricks Builder and WordPress components to fixed, supported versions.
- Disable unneeded themes/plugins and review their exposed REST API routes.
- Run the web application as an unprivileged account.
- Restrict writable locations, protect secrets, and monitor unusual PHP execution.
- Verify the fix by retesting the specific vulnerable path with a harmless request.

### Incident response

- Preserve the suspicious binary, service unit, logs, timestamps, and file hashes before removing artifacts where practical.
- Isolate the affected host if active mining or other malicious activity continues.
- Remove the unauthorized service and executable through a trusted recovery procedure.
- Rotate credentials and tokens that may have been exposed.
- Rebuild from a known-good image if host integrity cannot be trusted.
- Add monitoring for unusual service units, hidden executables, unexpected mining traffic, and excessive resource use.

### Threat intelligence

- Treat wallet addresses and process names as indicators, not attribution by themselves.
- Correlate multiple evidence sources and record the date and source of each claim.
- Preserve the exact original encoded value and show the transformation used to recover the candidate address.
- Distinguish evidence of a transaction association from proof of exclusive control by a threat actor.

This room demonstrates two complementary skills: finding the vulnerability that enabled entry, then using incident-response evidence to understand what the attacker ran, where its logs were stored, and how a wallet clue was tied to a known threat group.
