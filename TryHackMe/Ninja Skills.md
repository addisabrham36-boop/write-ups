# Ninja Skills: TryHackMe Walkthrough

Room: https://tryhackme.com/room/ninjaskills

Ninja Skills is a command-line practice challenge. The useful skill is learning to locate files, filter output, and combine small commands instead of manually opening every directory. The exact filenames and answers come from the room's provided filesystem, so use the active lab as the source of truth.

## 1. Start by orienting yourself

Open the room's terminal and check where you are:

    pwd
    ls -la

pwd prints the current directory. ls -la lists visible and hidden entries with permissions and ownership. If the expected files are not in the current directory, check the task instructions and navigate to the correct location.

## 2. Find files by name

The find command can search a directory tree:

    find . -type f -name "*.txt"

This finds regular files whose names end in .txt beneath the current directory. Use the path and filename pattern required by the task. Searching from the filesystem root can be slow and may produce permission errors, so begin in the relevant directory.

For a case-insensitive name match, find supports -iname. If the question asks for files with a particular extension, match that extension exactly and inspect the results.

## 3. Search file contents

grep searches text inside files. For example:

    grep -Rni "keyword" .

- -R searches recursively.
- -n prints line numbers.
- -i ignores case.

Replace keyword with the term provided by the task. If a file is binary or not readable, grep may not return useful output. Narrow the search path when possible.

## 4. Combine commands carefully

The pipe character sends one command's output to another command. For example:

    find . -type f | sort

find produces paths, and sort orders them. Another common pattern is to filter a listing with grep. Understand each stage separately before chaining them; otherwise, it becomes harder to identify which command caused an unexpected result.

## 5. Understand paths and hidden files

Relative paths are interpreted from the current directory. An absolute path begins at the filesystem root. The single dot represents the current directory, while two dots represent the parent directory. Files whose names begin with a dot are hidden by convention and appear when listing with ls -a or ls -la.

If a path is not found, check spelling, capitalization where relevant, and the current directory. Do not assume that a file is missing just because it is not visible in a normal listing.

## 6. Work through the challenge

Read each requirement and convert it into a search condition: filename, extension, path, text content, or file property. Run the smallest useful command, inspect the result, and then read the exact file required by the task. Do not make up filenames or flag values from another write-up.

If permissions prevent reading a file, note the error and check the intended lab instructions rather than trying to bypass access controls outside the exercise.

## What I took from this room

Command-line challenges become much easier when I break them into navigation, file discovery, content search, and output filtering. A few well-understood commands are more reliable than a long command copied without understanding it.