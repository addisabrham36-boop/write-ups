# Advent of Cyber 2024 - Day 16: The Wareville’s Key Vault grew three sizes that day.

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

## Overview & Investigation Walkthrough

1. What is the password for backupware that was leaked?

   If we search for the existing users, we find the password in one of the fields in plaintext.

   ```cmd
   az ad member list --filter "startsWith('wvusr', displayName)"
   ```

   ![Users](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_16_Users.png)

   ><details><summary>Click for answer</summary>R3c0v3r_s3cr3ts!</details>

2. What is the group ID of the Secret Recovery Group?

   To find the group ID we must list the existing groups with:

   ```cmd
   az ad group list
   ```

   ![Groups](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_16_Groups.png)

   ><details><summary>Click for answer</summary>7d96660a-02e1-4112-9515-1762d0cb66b7</details>

3. What is the name of the vault secret?

   With the credentials we just found, we can switch to that user so we have access to the keyvault.

   ```cmd
   az account clear
   az login -u <username> -p <password>
   ```

   ![Login](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_16_Login.png)

   Now we can list the existing keyvaults.

   ```cmd
   az keyvault list
   ```

   ![Keyvaults](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_16_Keyvaults.png)

   Now that we know the name we can look for existing secrets within the keyvault.

   ```cmd
   az keyvault secret list --vault-name warevillesecrets
   ```

   ![Secrets](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_16_Secrets.png)

   ><details><summary>Click for answer</summary>aoc2024</details>

4. What are the contents of the secret stored in the vault?

   With the secret name, we can see its contents.

   ```cmd
   az keyvault secret show --vault-name warevillesecrets --name aoc2024
   ```

   ![Flag](https://raw.githubusercontent.com/Kevinovitz/TryHackMe_Writeups/main/adventofcyber2024/Advent_of_Cyber_2024_Day_16_Flag.png)

   ><details><summary>Click for answer</summary>WhereIsMyMind1999</details>

5. Liked today's task? Check theExploiting Active Directoryroom to practice user and group enumeration in a similar yet different environment!

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the password for backupware that was leaked? | `R3c0v3r_s3cr3ts!` |
| What is the group ID of the Secret Recovery Group? | `7d96660a-02e1-4112-9515-1762d0cb66b7` |
| What is the name of the vault secret? | `aoc2024` |
| What are the contents of the secret stored in the vault? | `WhereIsMyMind1999` |

> **Note:** Flags are redacted as `flag{hidden}` in accordance with responsible disclosure guidelines. The step-by-step commands above provide the verified reproduction path.
