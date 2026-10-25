# Ninja Skills: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/ninjaskills

## Room Overview & Information

**Category:** Linux enumeration  
**Difficulty:** Beginner  
**Focus:** Finding files, inspecting ownership and permissions, searching file content, checking hashes, line counts, and executable bits.

This room gives a Linux machine with twelve files whose names are short and unhelpful: `8V2L`, `bny0`, `c4ZX`, `D8B3`, `FHl1`, `oiMO`, `PFbD`, `rmfX`, `SRSq`, `uqyw`, `v2Vb`, and `X1Uy`. The challenge is to answer several questions by inspecting the files efficiently rather than opening them one by one.

## Phase 1: Connect and inspect the directory

Start the target machine. If you use SSH, the room provides the credentials `new-user` / `new-user`. Use the address currently assigned by TryHackMe.

```bash
ssh new-user@MACHINE_IP
```

After connecting, verify your identity and working directory:

```bash
whoami
pwd
ls -la
```

- `whoami` verifies the logged-in user.
- `pwd` prints the current directory.
- `ls -la` shows all entries, including hidden files, with permissions, owner, group, size, and modification time.

Locate the twelve named files. If they are not in the current directory, use the path indicated by the room and then run the commands from that directory. Avoid running broad searches over the entire filesystem when the task provides a small target set.

## Phase 2: Find files by ownership group

### Question 1: “Which of the above files are owned by the best-group group (enter the answer separated by spaces in alphabetical order)?”

Use `ls -l` to inspect ownership and group columns:

```bash
ls -l 8V2L bny0 c4ZX D8B3 FHl1 oiMO PFbD rmfX SRSq uqyw v2Vb X1Uy
```

In long listing output, the owner appears before the group. Look for files whose group field is exactly `best-group`. List all matching filenames and sort them alphabetically. Submit the names separated by spaces as the question requests.

You can also query each file's metadata:

```bash
stat 8V2L
```

The `stat` output identifies owner and group explicitly. Use the actual lab output to determine which files match.

## Phase 3: Find a file containing an IP address

### Question 2: “Which of these files contain an IP address?”

Search the contents of the supplied files for text that resembles an IPv4 address:

```bash
grep -nE '([0-9]{1,3}\.){3}[0-9]{1,3}' 8V2L bny0 c4ZX D8B3 FHl1 oiMO PFbD rmfX SRSq uqyw v2Vb X1Uy
```

- `grep` searches text.
- `-n` includes line numbers.
- `-E` enables extended regular expressions.
- The expression matches four groups of one to three digits separated by dots.

This expression finds IPv4-shaped strings, but it does not validate every octet's range. For the room, inspect the matching line and confirm the value is an IP address. Submit the filename, not the address, if the question asks which file contains it.

## Phase 4: Match a SHA-1 hash

### Question 3: “Which file has the SHA1 hash of 9d54da7584015647ba052173b84d45e8007eba94?”

Compute the SHA-1 digest of each supplied file:

```bash
sha1sum 8V2L bny0 c4ZX D8B3 FHl1 oiMO PFbD rmfX SRSq uqyw v2Vb X1Uy
```

The command prints a hash followed by the filename. Find the line whose hash exactly matches:

```text
9d54da7584015647ba052173b84d45e8007eba94
```

The answer is the filename on that line. Do not confuse the file's contents with its hash. Hashes are calculated over the exact bytes, so changing a newline or editing the file would change the digest.

SHA-1 is used here because the room specifies it. For new security-sensitive integrity checks, prefer a modern hash such as SHA-256.

## Phase 5: Find the file containing 230 lines

### Question 4: “Which file contains 230 lines?”

Use `wc -l` to count newline characters in each file:

```bash
wc -l 8V2L bny0 c4ZX D8B3 FHl1 oiMO PFbD rmfX SRSq uqyw v2Vb X1Uy
```

The output shows a count and filename for each file. Find the entry with `230`. The command counts newline characters, so a file with a final unterminated line can behave differently from a visual line count. Follow the room's supplied file format and verify the matching file.

## Phase 6: Find the owner with UID 502

### Question 5: “Which file's owner has an ID of 502?”

The username may not make the numeric user ID obvious. Use `stat` or `ls -ln` to display numeric ownership.

```bash
ls -ln 8V2L bny0 c4ZX D8B3 FHl1 oiMO PFbD rmfX SRSq uqyw v2Vb X1Uy
```

The `-n` option displays numeric user and group IDs instead of resolving them to names. Look for UID `502` in the owner field and submit the associated filename.

To inspect a candidate individually:

```bash
stat -c '%n owner_uid=%u group_gid=%g' FILENAME
```

Replace `FILENAME` with the candidate. `%u` prints the numeric owner UID, and `%g` prints the numeric group ID.

## Phase 7: Find the file executable by everyone

### Question 6: “Which file is executable by everyone?”

File permissions are displayed in symbolic form, for example `-rwxr-xr-x`. The three permission triplets represent owner, group, and others. “Executable by everyone” means the execute bit is set for all three categories.

List the permissions:

```bash
ls -l 8V2L bny0 c4ZX D8B3 FHl1 oiMO PFbD rmfX SRSq uqyw v2Vb X1Uy
```

Look for an entry whose permission string has `x` in the owner, group, and other positions. A script may still fail to run if its interpreter is missing or its filesystem is mounted with execution disabled, but the question is about the permission bits.

You can verify a candidate with:

```bash
test -x FILENAME && echo executable || echo not-executable
```

This checks whether the current user can execute the file; it does not by itself prove that all users have execute permission. Inspect the mode string for the exact answer.

## Phase 8: Efficient workflow and troubleshooting

The exercise is designed to reward using the right command:
- `ls -l` for owner and group.
- `grep` for text patterns.
- `sha1sum` for a specified hash.
- `wc -l` for line counts.
- `ls -ln` or `stat` for numeric ownership.
- Permission bits from `ls -l` for executability.

If a command says a file does not exist, check spelling and case. Linux filenames are case-sensitive, so `FHl1` and `fhl1` are different names. If the hash does not match, ensure you are using the original file and not a modified copy.

## Key takeaways

Ninja Skills is a compact exercise in selecting efficient filesystem commands. Instead of reading every file manually, use metadata, pattern matching, hashes, line counts, numeric IDs, and permission bits to answer each question from direct evidence.

## Expanded filesystem enumeration workflow

### 1. Work from the supplied files

Start with the files or directory tree the room gives you. Confirm the current location and inspect entries, including hidden files:

```bash
pwd
ls -la
find . -maxdepth 3 -type f -print
```

`pwd` prints the current directory; `ls -la` shows hidden entries and metadata; `find` lists files within the chosen depth. Restrict searches to the challenge workspace rather than scanning unrelated system directories.

### 2. Search by metadata and content

The room may ask for a file based on name, extension, content, size, or metadata. Use the property stated in the question to choose the command. Examples include `find` with `-name`, `grep -R` for text, `wc -l` for line counts, and `stat` for file metadata.

```bash
find . -type f -name '*.txt' -print
grep -Rni 'SEARCH_TERM' .
wc -l path/to/file
stat path/to/file
```

Replace placeholders with the exact term or path from the task. `grep -Rni` searches recursively, ignores case, and prints line numbers; it can produce a lot of output, so narrow the search when possible.

### 3. Inspect archives and hashes

List archive contents before extracting. Record file hashes if the room asks for a digest, and ensure you hash the exact target file. Distinguish a checksum from a file size or inode number. Use a safe temporary directory for extraction and avoid overwriting existing files.

### 4. Interpret permissions and identifiers

Linux permission bits describe read, write, and execute rights for owner, group, and others. Numeric IDs identify users and groups in system metadata, but the mapping to names depends on the target's account database. Preserve leading zeros if the task treats an identifier as a string.

### 5. Common mistakes

- Forgetting hidden files.
- Searching the wrong starting directory.
- Confusing a filename with its full path.
- Counting bytes where the question asks for lines.
- Hashing a copy instead of the named original.
- Ignoring case sensitivity in a path.
- Treating one permission bit or metadata field as proof of malicious activity.

### 6. Complete every task from evidence

For each question, write the requested property and the exact command that reveals it. Verify the result by opening the corresponding file or checking its metadata. This room is driven by its supplied filesystem; do not invent file names or answers that are not present in your instance.
