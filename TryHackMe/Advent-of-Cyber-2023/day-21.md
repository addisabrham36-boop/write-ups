# Advent of Cyber 2023 - Day 21: DevSecOps

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
Indirect Poisoned Pipeline Execution in Gitea and Jenkins environments to access build runner secrets.

## Walkthrough
1. Inject arbitrary bash execution into the build stage of a project repository.
2. Check host system information:
   ```bash
   uname -r
   # 5.4.0-1029-aws
   ```
3. Extract Jenkins master secret key:
   ```bash
   cat /var/lib/jenkins/secret.key
   # 90e748eafdd2af4746a5ef7941e63272f24f1e33a2882f614ebfa6742e772ba7
   ```

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Linux kernel version of the Jenkins node? | `5.4.0-1029-aws` |
| Value found from `/var/lib/jenkins/secret.key`? | `90e748eafdd2af4746a5ef7941e63272f24f1e33a2882f614ebfa6742e772ba7` |
