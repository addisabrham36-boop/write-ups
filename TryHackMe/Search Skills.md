# Search Skills — TryHackMe

Room: https://tryhackme.com/room/searchskills

Search skills are useful in cybersecurity because the answer is often hidden in documentation, logs, source files, or public information rather than in a single tool. The trick is to narrow the search without accidentally filtering out the useful result.

## Search files and text

On Linux, `find` searches for files and `grep` searches text:

```bash
find . -type f -iname '*config*'
grep -Rni 'keyword' .
```

The first searches the current directory tree for filenames containing “config”, ignoring case. The second searches file contents recursively for a keyword and prints matching line numbers. I should run these from the lab's relevant directory and expect permission errors or binary-file noise sometimes.

## Search the web carefully

Search operators such as `site:`, `filetype:`, and quotation marks can narrow public results. I should compare sources and check dates, because search results can be stale or incomplete.

## What I took from this room

Searching is a skill, not just typing words into a box. I need to choose the right scope, use filters deliberately, and verify the result before relying on it. Recording the source also makes my work easier to reproduce later.