# Advent of Cyber Prep Track — TryHackMe

Room: https://tryhackme.com/room/adventofcyberpreptrack

This prep track is a set of short warm-up tasks for Advent of Cyber 2025. It covers password strength, basic malware triage, Linux and Windows commands, breach checking, Wi-Fi security, connected-app permissions, sensitive information in chatbot output, and identifying suspicious user agents. Each task is a small exercise, so I need to follow the interactive page and verify the answer from the result rather than reuse a guessed flag.

## Challenge 1: Password Pandemonium

The page tests password strength and checks whether the password appears in a leaked-password list. I need to satisfy all validation rules shown by the challenge, including length and character variety, while ensuring the password is not found in the breach list. I should use a unique practice password rather than reuse a real account password.

## Challenge 2: The Suspicious Chocolate.exe

The challenge provides a simulated file-scanning interface. I open the scan report and decide whether the sample should be treated as safe or malicious based on the result shown. The lesson is to treat a suspicious executable cautiously and use evidence from the scan, not its festive filename.

## Challenge 3: Welcome to the AttackBox

The room asks me to locate a welcome message in the AttackBox. The basic steps are:

```bash
ls
cd challenges/
cat welcome.txt
```

I list the current directory, enter the challenges directory, and read the file. If a command fails, I check the current directory and filename instead of guessing.

## Challenge 4: The CMD Conundrum

This task practises finding hidden files using Windows Command Prompt. The room's example workflow is:

```bat
dir
dir /a
type hidden_flag.txt
```

The `/a` option includes hidden files in the listing. I should use the directory and filename shown by the active task if they differ.

## Challenge 5: Linux Lore

This task asks me to locate a hidden message in a Linux home directory:

```bash
cd /home/mcskidy/
ls -la
cat .secret_message
```

The `-a` option shows hidden directory entries, including names beginning with a dot. The file path comes from the room scenario, and I should read the file only inside the provided lab.

## Challenge 6: The Leak in the List

The simulated breach checker asks me to check the example account `mcskidy@tbfc.com` and identify the domain marked as compromised. I should read the results shown by the tool and choose the matching entry. In real life, I would use a trusted breach-notification service and avoid submitting other people's private data without a legitimate reason.

## Challenge 7: WiFi Woes in Wareville

The challenge presents a router using default credentials and asks me to replace them with a strong password. The lesson is that default credentials should be changed immediately, management access should be restricted, and wireless security should use a modern supported protocol. This exercise belongs only in the provided simulation, not on a network I do not own.

## Challenge 8: The App Trap

The task asks me to inspect connected applications and revoke access from the suspicious app with excessive permissions. I should compare the permissions with what the app needs to do. In a real account, I would review connected-app permissions through the provider's official security settings.

## Challenge 9: The Chatbot Confession

I read the simulated chatbot conversation and identify messages that reveal sensitive information, such as internal URLs or passwords. The point is to recognise that generated output can disclose data if the system is given access to it or is not designed with appropriate controls. I should select only the messages supported by the evidence in the task.

## Challenge 10: The Bunny's Browser Trail

The final challenge asks me to find an unusual user-agent string in web logs. I compare the entries with common browser user agents and select the anomalous one shown by the room. A user-agent string can be forged, so it is a useful clue rather than proof of an attack by itself.

## What I took from this room

The prep track mixes basic security habits with command-line practice. I need to read the task carefully, use the supplied environment, and base each answer on the evidence on the page. The room asks for flags in its tasks, but I am replacing each actual flag in this public write-up with `flag{hidden}` rather than exposing the answer values.