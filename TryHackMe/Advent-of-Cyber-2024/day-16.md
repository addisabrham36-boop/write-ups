# Advent of Cyber 2024 - Day 16: Azure

{% embed url="https://tryhackme.com/room/adventofcyber2024" %}

The Wareville's Key Vault

## Background

Microsoft Azure cloud infrastructure secures application secrets, encryption keys, and TLS certificates using Azure Key Vault. Access policies and Azure Role-Based Access Control (RBAC) dictate who can read or manage vault objects.

When cloud credentials or Managed Identity tokens are exposed through application vulnerabilities (such as SSRF or XXE), attackers can authenticate via the Azure CLI (`az login`) or Azure REST APIs to enumerate vaults and extract credentials.

## Walkthrough

**Step 1 — Authenticate with Disclosed Credentials**

Authenticate to Azure using credentials recovered from earlier challenges:
```bash
az login --service-principal -u <client-id> -p <client-secret> --tenant <tenant-id>
```

**Step 2 — Enumerate Azure Key Vaults**

List accessible vaults across resource groups:
```bash
az keyvault list --query "[].{Name:name, ResourceGroup:resourceGroup}" -o table
```
Discovered vault: `aoc2024`.

**Step 3 — Retrieve Secret Values**

Query secrets stored within `aoc2024`:
```bash
az keyvault secret list --vault-name aoc2024 -o table
az keyvault secret show --vault-name aoc2024 --name secret-password --query "value" -o tsv
```
The secret value is `WhereIsMyMind1999`. Correlating with Day 5 provides admin password `R3c0v3r_s3cr3ts!` and UUID `7d96660a-02e1-4112-9515-1762d0cb66b7`.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the name of the Azure Key Vault? | `aoc2024` |
| What secret password was retrieved from the Key Vault? | `WhereIsMyMind1999` |
| What was the admin password recovered during the earlier XXE stage? | `R3c0v3r_s3cr3ts!` |
| What is the target secret UUID? | `7d96660a-02e1-4112-9515-1762d0cb66b7` |
