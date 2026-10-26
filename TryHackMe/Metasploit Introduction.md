# Metasploit: Introduction

{% embed url="https://tryhackme.com/room/metasploitintro" %}

An introduction to the Metasploit Framework, the world's most widely used penetration testing framework. This guide covers initialization, module categories, msfconsole navigation, and target configuration.

---

## 1. What is Metasploit?

Metasploit is an open-source exploitation framework maintained by Rapid7. It provides an extensive collection of exploits, payloads, auxiliary scanners, post-exploitation modules, encoders, and NOP generators.

---

## 2. Initialization & Architecture

To launch the Metasploit command-line interface:

```bash
msfdb init      # Initialize PostgreSQL database (recommended)
msfconsole      # Launch console
```

---

## 3. Module Hierarchy

| Module Type | Purpose | Example |
|---|---|---|
| **Auxiliary** | Scanning, port probing, fuzzing, and sniffing without exploitation | `auxiliary/scanner/portscan/tcp` |
| **Exploit** | Takes advantage of a system vulnerability to deliver a payload | `exploit/windows/smb/ms17_010_eternalblue` |
| **Payload** | Code that runs on the target after exploitation (e.g. reverse shell, Meterpreter) | `payload/windows/x64/meterpreter/reverse_tcp` |
| **Post** | Post-exploitation modules for dumping hashes, pivoting, or enumeration | `post/windows/gather/smart_hashdump` |
| **Encoder** | Obfuscates payloads to evade simple signature-based detection | `encoder/x86/shikata_ga_nai` |
| **NOP** | Generates no-operation sleds for buffer alignment | `nop/x86/opty2` |

---

## 4. Msfconsole Navigation

Common commands:
- `search <term>`: Search modules by name, CVE, or platform (e.g. `search apache`).
- `use <path>`: Select a module (e.g. `use exploit/windows/smb/ms17_010_eternalblue`).
- `info`: Display module author, description, CVE references, and options.
- `options` / `show options`: Display parameters required by the active module.
- `set <OPTION> <VALUE>`: Set a parameter for the current module context.
- `setg <OPTION> <VALUE>`: Set a parameter globally across all modules (e.g. `setg RHOSTS 10.10.10.5`).
- `back`: Deselect the current module and return to the main prompt.

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 2** | Starting the Metasploit Framework | `msfconsole` |
| **Task 3** | Module type used to find a wider range of vulnerabilities / scanners | `auxiliary` |
| **Task 3** | Module type used to execute a payload against a target | `exploit` |
| **Task 3** | Module type that facilitates communication between attacker and target | `payload` |
| **Task 4** | How would you search for a module related to Apache? | `search apache` |
| **Task 4** | How would you select module `exploit/windows/smb/ms17_010_eternalblue`? | `use exploit/windows/smb/ms17_010_eternalblue` |
| **Task 4** | How would you set global RHOSTS to `10.10.10.5`? | `setg RHOSTS 10.10.10.5` |
