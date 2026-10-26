# Advent of Cyber 2022 - Day 3: OSINT

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Open Source Intelligence (OSINT) techniques to investigate domain registrations (`whois`) and publicly exposed source code repositories (GitHub) for leaked credentials.

## Walkthrough
1. Run a `whois` lookup on the target domain `santagift.shop`:
   ```bash
   whois santagift.shop | grep -i "Registrar"
   ```
   The registrar is returned as `Namecheap Inc.`
2. Search GitHub for public repositories associated with `santagift.shop` or the Santa Gift company.
3. Locate the repository `santagift/website` and browse the commit history.
4. An old commit reveals hardcoded database credentials in `config.php`:
   ```php
   <?php
   $db_user = "santa_admin";
   $db_pass = "S4nt4_Cl4us_P4ssw0rd!";
   // Flag: {THM_OSINT_WORKS}
   ?>
   ```

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the name of the Registrar for the domain santagift.shop? | `Namecheap Inc.` |
| In the GitHub repository, open the file containing sensitive credentials. What is the flag? | `{THM_OSINT_WORKS}` |
| What is the name of the file containing passwords? | `config.php` |
