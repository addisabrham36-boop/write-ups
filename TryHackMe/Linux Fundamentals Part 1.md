# Linux Fundamentals Part 1: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/linuxfundamentalspart1

## Room Overview & Information

**Category:** Linux fundamentals  
**Difficulty:** Beginner  
**Focus:** Linux history, terminal commands, users, directory navigation, reading files, searching logs, background jobs, and output redirection.

Linux is common on servers, cloud infrastructure, containers, and security tools. The terminal is a way to issue commands to the shell, which starts programs and connects their input and output. A command should be understood as a program plus its arguments, not as a magic phrase to copy.

## Task 1: Introduction

The room introduces Linux and its use in cybersecurity. Linux is an operating-system family built around the Linux kernel and a large collection of user-space tools.

### Question: “Research: What year was the first release of a Linux operating system?”

**Answer:** **1991**.

Linus Torvalds announced the Linux kernel in 1991. Modern Linux distributions combine the kernel with system tools, libraries, package managers, and applications.

## Task 2: A Bit of Background on Linux

### Question: “If we wanted to output the text "TryHackMe", what would our command be?”

**Answer:**

```bash
echo "TryHackMe"
```

The `echo` command prints its arguments to standard output. Quotes keep the text together as one argument and are useful when the text contains spaces.

## Task 3: Interacting With Your First Linux Machine

Start the room's deployed Linux machine and open its terminal. The exercise uses an account and a simple directory tree.

### Question: “What is the username of who you're logged in as on your deployed Linux machine?”

**Answer:** `tryhackme`.

Check the active user with:

```bash
whoami
```

The command prints the effective username. Always verify the current environment instead of assuming the account name.

### Question: “On the Linux machine that you deploy, how many folders are there?”

**Answer:** **4**.

Use `ls` to inspect the provided directory. If the task refers to the exercise folders, count those entries in the active machine. Directory contents can vary if you are in the wrong location.

### Question: “Which directory contains a file?”

**Answer:** `folder4`.

Inspect the folders and identify which one contains the supplied file. Use:

```bash
ls -la
find . -maxdepth 2 -type f
```

The first command includes hidden entries; the second lists files within the current directory and one level below it.

### Question: “What is the contents of this file?”

**Answer:** `Hello World`.

Read the file with `cat` or `less`. The task expects the exact text shown by the supplied file.

### Question: “Use the cd command to navigate to this file and find out the new current working directory. What is the path?”

**Answer:** `/home/tryhackme/folder4`.

Navigate to the directory and check the current path:

```bash
cd /home/tryhackme/folder4
pwd
```

- `cd` changes the current directory.
- `pwd` prints the absolute path of the current working directory.

## Task 4: Running Your First Few Commands

### Question: “Use grep on "access.log" to find the flag that has a prefix of "THM". What is the flag?”

Search the log for the flag prefix:

```bash
grep 'THM' access.log
```

The room's log contains a flag. It is intentionally redacted in this public write-up as `THM{hidden}`. The important technique is using grep to search a file for a string rather than reading the entire log manually.

Useful options include:
- `-n` to show line numbers.
- `-i` for case-insensitive matching.
- `-E` for extended regular expressions.

## Task 5: Interacting With the Filesystem

### Question: “If we wanted to run a command in the background, what operator would we want to use?”

**Answer:** `&`.

Appending an ampersand to a command asks the shell to start it as a background job:

```bash
sleep 60 &
jobs
```

The example starts a harmless sleep process in the background and then lists shell jobs. The exact behaviour depends on the shell.

### Question: “If I wanted to replace the contents of a file named "passwords" with the word "password123", what would my command be?”

**Answer:**

```bash
echo password123 > passwords
```

The single greater-than operator redirects standard output to the file and overwrites its existing contents. Be careful: it destroys the previous file contents immediately.

### Question: “Now if I wanted to add "tryhackme" to this file named "passwords" but also keep "passwords123", what would my command be”

**Answer:**

```bash
echo tryhackme >> passwords
```

The double greater-than operator appends output to the end of the file without replacing existing content. In the room's question, the point is to keep the previous line while adding a new one.

## Command reference

| Command or operator | Purpose |
|---|---|
| `whoami` | Display the current username |
| `pwd` | Display the current working directory |
| `ls -la` | List files, including hidden entries |
| `cd PATH` | Change directory |
| `cat FILE` | Print a file |
| `grep TEXT FILE` | Search file contents |
| `&` | Run a command in the background |
| `>` | Redirect output and overwrite a file |
| `>>` | Redirect output and append to a file |

## Troubleshooting

- **No such file or directory:** check the current directory with `pwd` and confirm the filename's spelling and case.
- **Wrong folder count:** make sure you are viewing the directory described by the task.
- **Flag not found:** check the file path and search for the exact prefix used by the question.
- **File contents disappeared:** remember that `>` overwrites, while `>>` appends.

## Key takeaways

The room teaches the first practical Linux habits: check who you are, navigate with `cd`, confirm location with `pwd`, read files, search logs with `grep`, and understand background execution and redirection. These small skills become essential when analysing logs or working in a security lab.

## Expanded practical command guide

### 1. Confirm the environment

Start the room's Linux machine and open the terminal provided by the environment. Record the current user and directory:

```bash
whoami
pwd
uname -a
```

`whoami` identifies the effective username, `pwd` prints the working directory, and `uname -a` reports kernel and system information. The exact output depends on the lab image.

### 2. Navigate the filesystem

Linux uses a directory tree rooted at `/`. Common directories include `/home` for user home directories, `/etc` for configuration, `/var/log` for many logs, `/tmp` for temporary files, and `/bin` or `/usr/bin` for executable programs.

```bash
ls
ls -la
cd /etc
pwd
cd ~
```

`ls -la` shows hidden files and permissions; `cd` changes the directory; `~` refers to the current user's home directory. Use `cat FILE` to print a small text file and `less FILE` to inspect a longer one without flooding the terminal.

### 3. Create and manage files

```bash
mkdir practice
cd practice
touch notes.txt
printf '%s\n' 'practice note' > notes.txt
cat notes.txt
cp notes.txt notes-copy.txt
mv notes-copy.txt renamed.txt
```

These commands create a directory and file, write a line, display it, copy it, and rename the copy. The `>` redirection overwrites a file, while `>>` appends. Be careful with redirection because it can destroy existing content.

### 4. Users, groups, and permissions

Use `id` to inspect group membership and `ls -l` to read file permissions. The permission string describes read, write, and execute access for the owner, group, and others. `chmod` changes permissions; do not use broad permissions such as 777 as a routine fix.

### 5. Processes and help

```bash
ps aux
top
man ls
ls --help
```

`ps aux` lists processes, `top` gives a live view, and `man` or `--help` explains command options. If a command is unfamiliar, inspect its help rather than running it blindly.

### 6. Search and inspect files

Use `find` for filesystem searches and `grep` to search text:

```bash
find /home -type f -name '*.txt' 2>/dev/null
grep -Rni 'keyword' /path/to/directory
```

These commands can produce large output. Restrict the starting directory and use only paths relevant to the task. Permission errors do not mean the file does not exist.

### 7. Complete room questions

For each task, run the requested command in the room's terminal and read the exact output. Pay attention to hidden files, absolute versus relative paths, and case sensitivity. Do not paste in answers from a different VM when the task depends on the current filesystem.

### 8. Security takeaway

Linux commands are powerful because they operate directly on files, processes, and permissions. Verify the current directory and target path before destructive actions, use least privilege, and understand redirection and permissions before changing system state.
