# Advent of Cyber 2023 - Day 20: DevSecOps

{% embed url="https://tryhackme.com/room/adventofcyber2023" %}

## Background
DevSecOps and CI/CD security: Identifying **Poisoned Pipeline Execution (PPE)** in GitLab and analyzing unauthorized merge requests.

## Walkthrough
1. Review commit history and merge requests on the GitLab instance.
2. The malicious merge was committed by `@badsecops`.
3. Inspecting `.gitlab-ci.yml` reveals an injected command defacing the public Advent Calendar site running on port `9081` using `apache`.
4. Defaced page displays: `FROSTLINGS RULE`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| Handle of developer responsible for merge? | `@badsecops` |
| Port where defaced site server is running? | `9081` |
| Server type malicious server is running on? | `apache` |
| Message Frostlings left on the defaced site? | `FROSTLINGS RULE` |
