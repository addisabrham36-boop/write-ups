# Advent of Cyber 2024 - Day 1: OPSEC

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

OPSEC failures leave digital breadcrumbs — metadata, commit history, and embedded URLs can unmask even careful threat actors.

## Background

Operational Security (OPSEC) is the process of protecting information that could be used against you. Threat actors often make mistakes that expose their identity — leaving metadata in files, committing code with personal emails, or embedding infrastructure URLs directly in their malware.

`exiftool` is a powerful utility for reading, writing, and editing metadata in a wide range of file types. Even a seemingly innocent MP3 downloaded from a sketchy converter site can carry embedded author info, GPS coordinates, or URL references back to C2 infrastructure.

GitHub commit history is another goldmine for investigators. Every commit carries an author name and email. Even if a threat actor later scrubs a repository, historical commits may remain indexed by search engines or cached by third-party services, exposing the operator's true identity.

## Walkthrough

**Step 1 — Investigate the suspicious YouTube-to-MP3 site**

Navigate to the site presented in the room and download the suspicious file linked there. Notice the site offers a file that's a bit too convenient — it contains embedded malware.

**Step 2 — Extract metadata with exiftool**

```bash
exiftool <downloaded-file>
```

Among the output fields, look for a `URL` or custom metadata entry pointing back to attacker infrastructure:

```
URL : http://papash3ll.thm/data
```

That's the C2 URL embedded directly in the malware's metadata — a classic OPSEC failure.

**Step 3 — Hunt the author on GitHub**

Search GitHub for the embedded domain or any other strings found in the file. We're looking for a repository tied to this malware. The profile that surfaces belongs to `Mayor Malware`.

**Step 4 — Count the commits**

Open the repository and check the commit history. There is exactly `1` commit, which also leaks the author's identity through the commit metadata.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the C2 URL embedded in the malware metadata? | `http://papash3ll.thm/data` |
| Who is the actor behind the malware? | `Mayor Malware` |
| How many commits does the malicious repository have? | `1` |
