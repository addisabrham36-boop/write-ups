# Ninja Skills — TryHackMe

Room: https://tryhackme.com/room/ninjaskills

This room is a reminder that the Linux command line is often about combining small tools rather than memorising one giant command. The usual tasks involve finding files, searching through text, and understanding where information is stored.

## Finding files

The `find` command searches directory trees. For example, in a lab directory I can search for files by name:

```bash
find . -type f -name '*.txt'
```

The dot means the current directory, `-type f` restricts the results to regular files, and `-name` matches the filename pattern. I should start from the directory specified by the task rather than searching the whole machine without a reason.

## Searching file contents

```bash
grep -Rni 'search_term' .
```

This searches recursively, ignores case, and prints line numbers for matching text. It can be useful when the task hints that a value is hidden inside one of several files. If there are permission errors, I should check whether I am in the right location and whether the files are meant to be readable.

## Combining commands

The shell lets me pass output from one command into another with pipes. This is useful, but I should understand each part of a pipeline. For example, filtering a list with `grep` may hide lines that matter if the search term or case is wrong.

## What I took from this room

Linux enumeration rewards careful reading and small, targeted searches. I should use the room's clues to narrow the location, search by filename or content, and check the results rather than blindly copying a long command. Exact answers should come from the files in the lab, not from guesses in a write-up.