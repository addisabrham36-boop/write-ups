# Linux Fundamentals Part 1: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/linuxfundamentalspart1

## Room Overview & Information

**Category:** Linux fundamentals  
**Difficulty:** Beginner  
**Focus:** Linux shell, filesystem navigation, reading files, searching, shell operators, and basic system interaction.

Linux is common on servers, cloud infrastructure, containers, and security tools. The terminal is a way to issue commands to the shell, which starts programs and connects their input and output. A command should be understood as a program plus its arguments, not as a magic phrase to copy.

## Phase 1: Open a terminal and understand the prompt

A prompt may show the username, hostname, current directory, and shell. The exact appearance varies by distribution and shell configuration.

Check the current identity and directory:

```bash
whoami
pwd
id
```

- `whoami` prints the effective username.
- `pwd` prints the current working directory.
- `id` shows user and group identifiers.

The tilde (`~`) usually represents the current user's home directory.

## Phase 2: Navigate the filesystem

```bash
ls
ls -la
cd /etc
cd ..
cd ~
```

- `ls` lists visible entries.
- `-l` shows details such as permissions, owner, size, and modification time.
- `-a` includes hidden entries whose names begin with a dot.
- `cd` changes the current directory.
- `..` means the parent directory.
- `~` means the home directory.

Absolute paths begin at the filesystem root, `/`. Relative paths are interpreted from the current directory. This distinction matters when a command says a file cannot be found.

## Phase 3: Read and search files

```bash
cat notes.txt
less /var/log/syslog
head -n 20 notes.txt
tail -n 20 notes.txt
grep -n "error" notes.txt
find . -type f -name "*.txt"
```

- `cat` prints a file.
- `less` lets you page through a file.
- `head` shows the first lines.
- `tail` shows the last lines.
- `grep -n` searches for a string and displays matching line numbers.
- `find` searches the directory tree for matching paths.

Use `sudo` only when the task requires elevated permissions. Do not change ownership or permissions just to silence an error without understanding the consequence.

## Phase 4: Files, directories, and permissions

Linux permissions distinguish read (`r`), write (`w`), and execute (`x`) for the owner, group, and others. For a file, execute permission allows it to be run as a program; for a directory, execute permission controls whether the user can traverse it.

```bash
stat notes.txt
chmod 640 notes.txt
```

`stat` inspects metadata. `chmod 640` sets owner read/write, group read, and no permissions for others. This is an example for a file that should not be executable; do not apply it indiscriminately to system files.

Create or copy files with:

```bash
mkdir practice
cp notes.txt practice/
mv practice/notes.txt practice/notes-copy.txt
```

These commands create a directory, copy a file, and rename/move the copy.

## Phase 5: Redirects and pipes

The shell can redirect output and connect commands:

```bash
command > output.txt
command >> output.txt
command 2> errors.txt
command1 | command2
```

- `>` overwrites a file with standard output.
- `>>` appends standard output.
- `2>` redirects standard error.
- `|` sends one program's standard output to another program's standard input.

Be careful with `>`: it overwrites the destination immediately. Use a temporary file or append mode when you need to preserve existing content.

## Phase 6: Users, processes, and system information

```bash
ps aux
top
uname -a
df -h
free -h
ip addr
ip route
```

These commands inspect running processes, a live process view, kernel/system details, disk usage, memory, interface addresses, and routes. They are common starting points for both troubleshooting and defensive investigation.

## Phase 7: Working through the room

Read each task carefully and use the terminal to inspect the exact path or value it asks about. If the room asks for a hidden file, include hidden entries with `ls -la`. If it asks for a command's output, run the command in the provided environment. Paths and file contents may differ across instances, so do not invent output.

## Troubleshooting

- **Command not found:** check spelling and whether the tool is installed.
- **No such file or directory:** check `pwd`, path spelling, and case.
- **Permission denied:** inspect permissions and current identity before using `sudo`.
- **Unexpected output:** read the command's manual with `man COMMAND` or use `COMMAND --help`.
- **File overwritten:** remember that `>` replaces a file; use `>>` to append.

## Key takeaways

Linux fundamentals are the foundation for security work. Know where you are, understand file permissions, read the output of commands, and use pipes and redirection deliberately. These habits reduce mistakes when investigating logs or working in a lab.