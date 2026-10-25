# TryHack3M: Bricks Heist — TryHackMe Walkthrough

Room: https://tryhackme.com/room/tryhack3mbricksheist

This room combines web reconnaissance with Linux incident investigation. The scenario describes a compromised WordPress-related server and a suspected cryptocurrency miner. The goal is not just to gain access; it is to identify evidence of the compromise and report what happened.

## 1. Start the lab and configure the hostname

Start the target machine and wait for its services to finish booting. Record the IP shown by TryHackMe. The room instructs you to map the hostname bricks.thm to that IP in your local hosts file.

On a Linux AttackBox, edit /etc/hosts with administrative permission and add a line in this form:

    MACHINE_IP bricks.thm

Replace MACHINE_IP with the actual address assigned to your instance. Do not use an address from an old guide. Verify that the hostname resolves to the target before continuing.

## 2. Enumerate exposed services

Start with a service scan and save the results:

    nmap -sC -sV -oN bricks-nmap.txt MACHINE_IP

Then inspect the web service in a browser and use the room's hostname. Record the application, visible pages, and any unusual paths. If a directory scan is required by the task, run it only against this target and review discovered paths manually.

The scan tells you what is reachable, not whether the service is vulnerable. Use the web evidence and task clues to determine which application or component needs further inspection.

## 3. Inspect the web directory and hidden text file

One room question asks for the content of a hidden .txt file in the web folder. Treat this as an evidence-discovery task. Identify the relevant web root or directory from the machine's exposed application and the room clues, then locate the file without guessing its content.

Once you have authorised shell access, inspect directory entries including hidden files and read the target file. Use commands such as ls -la and cat on the specific path found in the lab. Do not publish the secret value in this public note; record the method and submit the exact value in the room.

## 4. Identify the suspicious process

A cryptocurrency miner may appear as a process with an unusual name, unexpected resource use, or a path that does not match normal system software. Inspect the process list and examine the command line, parent process, user, executable path, and start time.

Useful Linux investigation commands include:

    ps auxf
    top
    pgrep -a -f miner
    systemctl --type=service --all

The last command lists systemd services. These are general discovery commands; use the output from the target rather than assuming the suspicious process has a particular name.

A process name alone is not enough to establish maliciousness. Correlate it with the service that launched it, its executable path, network connections, and the room's other evidence.

## 5. Trace the service and miner log

If the process is managed by systemd, inspect the associated service unit and its status. The unit may reveal the executable path, arguments, restart behaviour, and service user. Review relevant logs to determine when the process started and what it did.

For example, after identifying the actual service name, use systemctl status SERVICE_NAME and journalctl -u SERVICE_NAME. Replace SERVICE_NAME with the service found on the target. Do not assume a service name from a different machine.

The room also asks for the log filename associated with the miner instance. Search the relevant application and service directories based on the process command line and service configuration. Read metadata and context before concluding that a file is the correct log.

## 6. Find and research the wallet address

The miner's configuration or log may contain a cryptocurrency wallet address. Record the address exactly as shown in the lab and compare it with the task's evidence. A wallet string should be treated as an indicator, not proof by itself that a particular actor operated the server.

The final question asks about a threat group associated with transactions involving the wallet. Use the room's evidence and the cited threat-intelligence source to establish the association. Check the exact wording and distinguish between an observed transaction link and a definitive attribution. Do not infer a group solely from a wallet format.

## 7. Build a defensible incident timeline

A useful timeline links the exposed service, the suspicious process, its service configuration, the miner log, and the wallet evidence. Record which items are direct observations and which are interpretations. If the room provides timestamps, check their timezone and order before describing the sequence.

For remediation, recommend removing the unauthorised persistence mechanism, rebuilding from a trusted image if integrity is uncertain, rotating exposed credentials, patching the entry point, restricting service permissions, and monitoring for recurrence. Do not simply kill the process and assume the compromise is resolved.

## What I took from this room

Bricks Heist is as much an incident investigation as a penetration test. Service enumeration provides the entry point, but process, service, log, and wallet evidence are what explain the compromise. I use the live target to verify every value rather than copying machine-specific answers into the walkthrough.