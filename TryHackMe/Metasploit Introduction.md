# Metasploit Introduction: TryHackMe Walkthrough

Room: https://tryhackme.com/room/metasploitintro

Metasploit is a framework for organising security tests. It includes modules for auxiliary tasks, exploit attempts, payloads, and post-exploitation. In a controlled lab, it helps demonstrate how a known weakness can be tested and why patching and configuration matter.

## 1. Start with reconnaissance

Do not begin by launching random exploits. First identify the target and the services it exposes. Use the scan method provided by the room and note the service, version, and relevant port. A vulnerability must match the actual target software and configuration.

A common Nmap pattern is:

    nmap -sC -sV -oN target-scan.txt TARGET_IP

Replace TARGET_IP with the current room address. Save the output so you can compare it with later steps. Version strings are clues, not proof of vulnerability; vendors sometimes backport fixes without changing the apparent upstream version.

## 2. Start Metasploit and understand modules

Launch the console with msfconsole in the provided environment. Metasploit modules commonly include:

- **Auxiliary:** scanning, enumeration, and other supporting tasks.
- **Exploit:** attempts to trigger a vulnerability.
- **Payload:** code or functionality that runs after a successful exploit.
- **Post:** actions performed after access has been established.
- **Encoder and NOP-related modules:** support specific payload or exploit requirements.

Use the module search and help features to understand what a module does. Read its description, required options, target support, and references before running it.

## 3. Configure a module carefully

A safe lab workflow is:

1. Identify the exact module relevant to the room.
2. Read its description and option list.
3. Set only the target and options required by the exercise.
4. Confirm the target address and port before execution.
5. Run the module only against the authorised lab machine.
6. Record the output and verify whether the expected condition occurred.

Metasploit's options differ by module. Do not copy a command from one exploit and assume another module uses the same fields. Check the current module's required options and default values.

## 4. Understand payloads and sessions

An exploit and a payload are different concepts. The exploit attempts to trigger a weakness; the payload describes the action or code that runs if exploitation succeeds. Some modules create a session, while others simply check a condition or return information.

A message saying that a module completed does not always mean a session was created. Verify the result through the module output and, when applicable, the session list. If the room expects a shell, use only the session created for that lab target.

## 5. Troubleshooting

- **No route or timeout:** check the lab machine state and VPN/AttackBox connection.
- **Connection refused:** the service may be stopped or the port may be wrong.
- **Exploit reports no target:** re-check service detection and module target requirements.
- **Exploit fails:** the version may not be vulnerable, the target may be patched, or a required option may be missing.
- **No session appears:** a module may have failed before payload execution, or the payload may not be compatible.

Do not treat repeated exploit attempts as a substitute for verifying the target. In a real assessment, agree on scope and impact limits before testing modules that can crash a service or modify data.

## 6. Post-exploitation and reporting

If the lab explicitly grants a session, perform only the actions required by the exercise. Record what access was achieved and what evidence demonstrates it. Avoid collecting unrelated files or sensitive data.

A professional report should include the affected service, the weakness, reproducible evidence, impact, and a specific mitigation. Patching, disabling unnecessary services, reducing privileges, and monitoring exploitation attempts are common controls, depending on the finding.

## What I took from this room

Metasploit is a framework, not a replacement for understanding the target. Reconnaissance and module selection come first; careful option checking and evidence verification come next. The same workflow helps avoid testing the wrong service or claiming success when no exploit actually worked.