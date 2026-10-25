# Linux Fundamentals Part 1: TryHackMe Walkthrough

Room: https://tryhackme.com/room/linuxfundamentalspart1

Linux is common in servers, cloud environments, security tools, and embedded devices. This room introduces the terminal and the commands needed to navigate a filesystem, inspect files, and understand the command line without relying entirely on a graphical desktop.

## 1. Open the terminal and understand commands

A shell reads commands and runs programs. A command may take options and arguments. For example, ls -la /etc lists the contents of /etc with detailed information, including hidden entries. The command is the program, -la are options, and /etc is the path being listed.

Try the following in the lab:

    whoami
    pwd
    uname -a

whoami prints the current username. pwd prints the current working directory. uname -a displays kernel and system information. The result is specific to the lab machine.

## 2. Navigate the filesystem

Linux uses one directory tree rooted at /. Common directories include /home for user home directories, /etc for system configuration, /var for changing data such as logs, and /tmp for temporary files.

    ls
    ls -la
    cd /tmp
    pwd
    cd ~

ls lists visible entries. ls -la also shows hidden entries and details. cd changes the current directory. The tilde represents the current user's home directory.

A name beginning with a dot is hidden by convention, not encrypted. Use ls -la when you need to check for such entries.

## 3. Read and search files

- cat filename.txt displays a short file.
- less filename.txt lets you scroll through a longer file.
- head filename.txt displays the beginning.
- tail filename.txt displays the end.
- file filename.txt identifies the file type.

Replace the example filename with one that exists in the room.

To search text, use grep -n "pattern" filename.txt. To search for matching paths under the current directory, use find . -name "*.txt". grep searches file contents; find searches filesystem paths. If a command returns no output, there may simply be no matches.

## 4. Create and move files

Commands such as touch, mkdir, cp, and mv create files, create directories, copy files, and move or rename paths. Practise in a directory you control. Before using mv or deleting anything, check the source and destination carefully; a typo can affect the wrong files.

## 5. Permissions and elevated privileges

Linux permissions control who can read, write, or execute files. Use ls -l to inspect permission bits and ownership, and id to see the current user and groups. sudo can run an authorised command with elevated privileges. It should not be used automatically to get around every permission error. First determine why access is denied.

## 6. Help and command discovery

man ls opens the manual page for ls. Many programs also support --help. The which command can show the executable found for a command name, although aliases and shell functions can affect command resolution.

## 7. Working through the room

Read the task prompt, identify the directory or file it refers to, and run the relevant command. If a question asks for a hidden file, remember to list hidden entries with ls -la. If a command fails, check the current directory with pwd, confirm the filename with ls, and read the exact error.

Do not assume the same files exist on your own machine. Use the AttackBox or lab environment provided by TryHackMe and record the actual result from that environment.

## What I took from this room

The command line becomes manageable when I understand paths, listing, reading, searching, and permissions. These fundamentals are the basis for later Linux enumeration and incident-response work.