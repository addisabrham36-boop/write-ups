# Linux Fundamentals Part 1 — TryHackMe

Room: https://tryhackme.com/room/linuxfundamentalspart1

Linux is widely used on servers, security tools, and cloud systems. The terminal lets me interact with the system by entering commands, which can be faster and more precise than navigating everything through a graphical interface.

## Basic navigation

```bash
pwd
ls
cd /path/to/directory
```

`pwd` prints the current directory, `ls` lists its contents, and `cd` changes directories. The path in the example is a placeholder, so I replace it with a real path from the lab.

## Reading files and getting help

```bash
cat notes.txt
less notes.txt
man ls
```

`cat` prints a file, `less` lets me read longer files page by page, and `man` opens the manual page for a command. If I forget an option, checking the manual is better than guessing.

## Users and permissions

Linux uses users and groups to control access. Commands such as `id` show the current identity and group memberships, while `ls -l` shows file permissions. I should understand the permission bits before changing them; making everything writable is not a security solution, despite its popularity among people in a hurry.

## What I took from this room

The terminal is a collection of small tools that each do a specific job. Learning navigation, file inspection, help pages, and permissions gives me a foundation for later enumeration and security tasks.