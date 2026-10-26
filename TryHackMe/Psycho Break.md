# Psycho Break

{% embed url="https://tryhackme.com/room/psychobreak" %}

The Evil Within themed CTF. Mix of web enumeration, steganography, cipher decoding, FTP, binary reverse engineering, and SSH access. Three open ports: FTP (21), SSH (22), HTTP (80).

---

## Recon

```bash
nmap -sC -sV -p- MACHINE_IP
```

| Port | Service |
|---|---|
| 21 | FTP (vsftpd) |
| 22 | SSH (OpenSSH) |
| 80 | HTTP (Apache) |

3 open ports. OS: Ubuntu.

---

## Web — Finding the Hidden Path

Visit `http://MACHINE_IP`. Check the page source — there's a comment:

```html
<!-- Sebastian sees a path through the darkness which leads to a room => /sadistRoom -->
```

Navigate to `http://MACHINE_IP/sadistRoom`. There's an image/hash on the page.

**Key to the locker room:** `532219a04ab7a02b56faafbec1a4c1ea`

Use this key to access `/lockerRoom`. We get a ciphertext:

```
Tizmg_nv_zxxvhh_gl_gsv_nzk_kovzhv
```

This is **Atbash cipher**. Decoded: `Allow_me_access_to_the_map_please`

Use that to find the keeper area. Continue exploring — find a text file whose name (without extension) is the next answer.

**Text file name (no extension):** `you_made_it`

---

## Steganography — The WAV File

Download the `.wav` file. It contains Morse code. Decode it to get FTP credentials.

**What the .wav says:** Morse decoded — it spells out the FTP info.

---

## FTP Access

```bash
ftp MACHINE_IP
# Username: joseph
# Password: intotheterror445
```

Download the files. One of them is a binary called `program` and a dictionary `random.dic`.

**Who is locked up in the cell?** `Joseph`
**FTP username:** `joseph`
**FTP password:** `intotheterror445`

---

## Cracking the Binary

Brute-force the binary with the dictionary:

```bash
while read LINE; do
  output=$(./program "$LINE" 2>/dev/null)
  echo "$LINE: $output"
done < random.dic
```

The key that works: `kidman`

The program outputs a sequence of numbers that map to phone keypad letters:
```
55 444 3 6 2 66 7 2 7 7 999 ... → KIDMANSPASSWORDISSOSTRANGE
```

**Key used by the program:** `kidman`
**Decoded message:** `kidmanspasswordissostrange`

---

## Getting user.txt

SSH in as `kidman`:

```bash
ssh kidman@MACHINE_IP
# password: KIDMANSPASSWORDISSOSTRANGE
cat user.txt
```

---

## Privilege Escalation

Check `/etc/crontab` and running processes. There's a root cron job. The win condition involves deleting user `ruvik`:

```bash
deluser ruvik
```

After that the root flag appears or becomes accessible:

```bash
cat /root/root.txt
```

---

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| How many ports are open? | `3` |
| What is the OS? | `ubuntu` |
| Key to the locker room | `532219a04ab7a02b56faafbec1a4c1ea` |
| Key to access the map (Atbash decoded) | `Allow_me_access_to_the_map_please` |
| Text file name (no extension) | `you_made_it` |
| Who is locked in the cell? | `Joseph` |
| FTP username | `joseph` |
| FTP password | `intotheterror445` |
| Key used by the program | `kidman` |
| What do the crazy numbers mean? | `kidmanspasswordissostrange` |
| user.txt | `4C72A4EF8E6FED69C72B4D58431C4254` |
| How to defeat Ruvik | `deluser ruvik` |
| root.txt | `BA33BDF5B8A3BFC431322F7D13F3361E` |
