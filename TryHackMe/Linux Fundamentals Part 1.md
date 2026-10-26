# Linux Fundamentals Part 1

Welcome to the Linux Fundamentals Part 1 room walkthrough on TryHackMe!

{% embed url="https://tryhackme.com/room/linuxfundamentalspart1" %}

***

Linux Fundamentals Part 1 is an introductory lab introducing essential Linux operating system concepts, shell commands, filesystem navigation, reading files, searching logs with `grep`, and working with input/output redirection.

---

## 1. Background on Linux

Linux was created by Linus Torvalds in 1991 as a free, open-source UNIX-like operating system kernel.

* To output text directly to the terminal, we use the `echo` command:
  ```bash
  echo "TryHackMe"
  ```

---

## 2. Navigating the Filesystem

Once deployed, we log into the terminal session as user `tryhackme`.

### Basic System Check:
```bash
whoami
# Output: tryhackme

pwd
# Output: /home/tryhackme
```

### Exploring Directories:
```bash
ls -la
```
Listing the home directory reveals **4 folders**: `folder1`, `folder2`, `folder3`, `folder4`.

Checking inside each folder:
```bash
find . -maxdepth 2 -type f
```
Output shows that `folder4` contains a file:
```bash
cd /home/tryhackme/folder4
pwd
# Output: /home/tryhackme/folder4

cat note.txt
# Output: Hello World
```

---

## 3. Searching Files with `grep`

Log analysis often requires filtering out specific keywords from massive log files. Using `grep`, we search `access.log` for lines containing `"THM"`:

```bash
cd /home/tryhackme
grep "THM" access.log
```

### Output:
```text
flag{hidden}
```

---

## 4. File Redirection & Background Execution

### Running in the Background:
Appending `&` to any shell command executes it in the background:
```bash
sleep 60 &
jobs
```

### Overwriting vs Appending:
* **`>` (Overwrite):** Replaces the target file's contents:
  ```bash
  echo "password123" > passwords
  ```
* **`>>` (Append):** Appends to the end of the file preserving existing content:
  ```bash
  echo "tryhackme" >> passwords
  ```

---

## 🏁 Questions & Answers

| Task / Question | Answer |
| :--- | :--- |
| **Research: What year was the first release of a Linux operating system?** | `1991` |
| **If we wanted to output the text "TryHackMe", what would our command be?** | `echo "TryHackMe"` |
| **What is the username of who you're logged in as on your deployed Linux machine?** | `tryhackme` |
| **On the Linux machine that you deploy, how many folders are there?** | `4` |
| **Which directory contains a file?** | `folder4` |
| **What is the contents of this file?** | `Hello World` |
| **Use the cd command to navigate to this file and find out the new current working directory. What is the path?** | `/home/tryhackme/folder4` |
| **Use grep on "access.log" to find the flag that has a prefix of "THM". What is the flag?** | `flag{hidden}` |
| **If we wanted to run a command in the background, what operator would we want to use?** | `&` |
| **If I wanted to replace the contents of a file named "passwords" with the word "password123", what would my command be?** | `echo password123 > passwords` |
| **Now if I wanted to add "tryhackme" to this file named "passwords" but also keep "passwords123", what would my command be?** | `echo tryhackme >> passwords` |

***
