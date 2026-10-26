# Operating Systems

{% embed url="https://tryhackme.com/room/operatingsystems" %}

A practical and conceptual review of operating system architecture, security hardening, user permission models, bash history credential discovery, and local privilege escalation.

---

## 1. Operating System Hardening

Core OS security relies on least privilege, strong credential policies, and auditing:
- Enforcing multi-character passwords (uppercase, lowercase, numbers, symbols) resistant to dictionary attacks.
- Disabling unneeded background services and auditing world-readable configuration files.

---

## 2. Practical Investigation & Privilege Escalation

1. Connect to the target machine via SSH:
   ```bash
   ssh sammie@MACHINE_IP
   ```
2. Users frequently make operational errors, such as accidentally typing passwords directly into the terminal or saving them in plaintext command histories.
3. Checking Johnny's command history or world-readable files reveals Johnny's credentials:
   `happyHack!NG`
4. Switch to the root account using the discovered password:
   ```bash
   su - root
   # Enter password: happyHack!NG
   ```
5. Read the root flag from `/root/flag.txt`:
   `flag{hidden}`

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 3** | Password discovered from command history | `happyHack!NG` |
| **Task 3** | Content of root flag file `/root/flag.txt` | `flag{hidden}` |
