# Psycho Break: TryHackMe Walkthrough

Room: https://tryhackme.com/room/psychobreak

Psycho Break is a themed boot-to-root challenge based on *The Evil Within*. It combines service enumeration, web-source inspection, ciphers, OSINT, archive analysis, audio decoding, password cracking, and Linux privilege escalation. I solve it as a chain: each room on the map gives a clue needed for the next one.

## Task 1: Recon

### 1. Start the machine and scan it

Start the target and record the assigned IP. Run:

    nmap -sC -sV -O -oN psychobreak-nmap.txt TARGET_IP

Replace TARGET_IP with the live room address. The scan identifies open ports, service versions, and operating-system clues. The room's intended machine exposes three ports and runs Ubuntu, but confirm the result from your own instance because target configuration can change.

The scan identifies FTP, SSH, and a web service. Keep the output because it guides the next steps. Do not assume an exposed service is vulnerable just because it is present.

## Task 2: Web

### 2. Inspect the homepage source

Open the target's web page and inspect its source, not just the visible text. The source contains a comment pointing to the Sadist room path:

    /sadistRoom

Visit that path and inspect the links and messages. A hidden path in a source comment is a clue, not a security boundary.

### 3. Obtain the locker-room key

The Sadist room contains a link or interaction that reveals the locker-room key. Record the key from the active page and submit it to the room. After dismissing the pop-up, the page changes and presents the next step.

Use the key shown by the current instance. Do not confuse this key with the later map key or the Keeper key; they unlock different stages.

### 4. Decode the map message

The locker-room page shows this text:

    Tizmg_nv_zxxvhh_gl_gsv_nzk_kovzhv

This is an Atbash substitution. Each letter maps to the opposite end of the alphabet: A becomes Z, B becomes Y, and so on. Decoding the string gives the phrase used to access the map. Submit that phrase in the room's requested field and follow the map links to the Safe Heaven and Abandoned Room areas.

The lesson is to identify the cipher from the text structure and clue context, then verify the decoded phrase by using it in the intended lab workflow.

### 5. Find the Keeper key in Safe Heaven

Open Safe Heaven from the map and inspect its page source for additional comments or clues. The task points toward a directory-discovery step. Run a directory scan against that path only:

    gobuster dir -u http://TARGET_IP/SafeHeaven/ -w /path/to/wordlist.txt

Replace the wordlist path with one available in the AttackBox. Inspect discovered directories manually. The intended path leads to the Keeper page.

The Keeper page includes a timed challenge asking where an image was taken. Use reverse-image search on the supplied image and verify that the location matches the image itself. Submit the location in the lab to obtain the Keeper key. Record the key for the next room; do not substitute a key from another machine instance.

## Task 3: Help Mee

### 6. Enter the Abandoned Room

Use the Keeper key to access the Abandoned Room from the map. The page includes a timed escape section and a source-code comment that mentions something called shell. This is a clue to inspect how the page handles a request parameter.

Inspect the page source and requests in the browser. The challenge's input handling is vulnerable to a restricted command-execution path. Use only the lab's assigned target and the smallest harmless commands needed to discover the provided files.

### 7. Discover the hidden directory and archive

The room's hint points toward using a parent-directory listing. The important reasoning is that the page's command parameter does not accept every command equally, so a command that lists a parent path can reveal a directory not visible in the current location.

Once the hidden path is identified, access the files provided there and download the archive for local inspection inside the lab environment. Do not assume the archive's visible extension matches its actual format.

### 8. Extract the nested files

The archive includes a text note and a file named Table.jpg. Use file to identify the actual file type before opening it:

    file helpme.zip
    file Table.jpg

The supposed image is another ZIP archive. Extract it and inspect its contents. The nested archive contains an image and a WAV audio file. Read the text note to identify the person locked in the cell, then examine the audio clue.

### 9. Decode the WAV clue

The audio contains a Morse-code message. Use a Morse audio decoder or inspect the waveform and timing with an appropriate tool. The decoded word is the answer to the room's audio question. Verify the result by listening to the audio and comparing the decoded characters; do not guess from the story.

The archive chain matters: the file named Table.jpg is not an ordinary JPEG, and recognising its real type reveals the next artefacts.

### 10. Recover the FTP credentials

Use the credentials revealed by the previous clues to authenticate to the FTP service identified during reconnaissance. List the available files and download only the artifacts relevant to the challenge. Check each downloaded file with file before deciding how to analyse it.

The room provides a dictionary file and a small compiled program. Keep these files together in the lab workspace so the paths used in the next step are clear.

## Task 4: Crack it open

### 11. Identify the password-checking program

The compiled program accepts one candidate word and reports whether it is correct. Running it without an argument displays its usage. The dictionary file contains candidate values.

Inspect the files first:

    file program
    file random.dic
    strings -n 5 program | head -n 40

Strings output is only a clue; it does not reveal all program logic. The intended task is to test the supplied candidate list against the program rather than randomly guessing passwords.

### 12. Test the supplied wordlist

Use a controlled local loop to run the program against the dictionary entries. Keep the loop limited to the provided program and candidate list. The accepted candidate is the key used by the program, and the program then prints a sequence of long numbers.

Do not run unknown binaries outside the lab. In this challenge the executable and wordlist are provided as part of the intended exercise, but they should still be treated as untrusted files.

### 13. Decode the numeric message

The output is not ordinary decimal data. The numbers correspond to multi-tap input from an old mobile-phone keypad, sometimes called T9-style multi-tap encoding. Repeated digits select different letters on the same key. Decode the sequence using the keypad mapping and check that the result forms a meaningful phrase.

Submit the decoded phrase to the room. Record the transformation rather than only the answer, because it explains why the numbers appeared after the correct program key was supplied.

## Task 5: Go Capture The Flag

### 14. Obtain the user and root flags

Use the access path established in the previous tasks to obtain a shell on the target. Verify the current identity and system context:

    whoami
    id
    hostname
    pwd
    ls -la

Locate the user flag as directed by the room. The target also includes a final root-level task. Do not claim root access until the identity check confirms it.

The actual user and root flag values are deliberately not reproduced here. Obtain them from your own running instance and submit them in TryHackMe; in this public write-up, represent each as flag{hidden}.

### 15. Bonus: Defeat Ruvik

The bonus task refers to a clue about the string the_eye_of_ruvik and instructs the learner to remove the Ruvik account. This step requires root-level permissions. Follow the active room's hint, confirm the account and its home directory, and perform the requested action only on the lab target. Verify the resulting state rather than assuming the command succeeded.

## What I took from this room

Psycho Break is a good reminder that a CTF can combine many different skills: Nmap, source inspection, directory discovery, Atbash, reverse-image search, archive identification, Morse audio, wordlist testing, numeric keypad decoding, and Linux privilege escalation. Each stage supplies evidence for the next. The best way through it is to record clues carefully, verify each transformation, and keep the entire exercise within the assigned machine.