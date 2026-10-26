# Passive Reconnaissance: Practical TryHackMe Notes

Official room: https://tryhackme.com/room/passiverecon

## Room Overview & Information

**Category:** OSINT and reconnaissance  
**Objective:** Build an initial picture of a target from public sources without directly probing its services.

Passive reconnaissance uses information that already exists publicly, such as registration data, DNS records, certificate logs, public websites, repositories, and search-engine indexes. Its value depends on attribution and evidence quality: a result can be public yet stale, shared by several organisations, or unrelated to the intended target.

## Phase 1: Define the Scope

Record the organisation, exact domains, permitted sources, and any exclusions stated by the room. Similar domain names may belong to different organisations. Do not treat a public IP or discovered hostname as permission to scan it.

For every finding, record:
- The exact value found.
- Source URL, search query, or command.
- Collection date and time.
- Whether the source is primary or secondary.
- Confidence and any uncertainty.
- Why the finding matters to the assessment.

This makes the work reproducible and helps separate facts from assumptions.

## Phase 2: Search Public Sources

Useful sources include official websites, search engines, registrar records, public DNS, certificate-transparency services, public documentation, public repositories, and approved internet search services. Search operators can narrow results by site, file type, title, or phrase.

Examples:

```text
site:example.com
site:example.com filetype:pdf
site:example.com intitle:login
"exact phrase" site:example.com
```

Replace example.com with a domain explicitly in scope. A search result or snippet may be stale or incomplete; open the original source where permitted and confirm that it supports the conclusion.

## Phase 3: Domain and DNS Information

DNS records can reveal web hosts, mail services, nameservers, aliases, and verification strings. Common record types include:
- **A:** IPv4 address.
- **AAAA:** IPv6 address.
- **MX:** mail exchanger and preference.
- **NS:** authoritative nameserver.
- **TXT:** text used for verification or email-security policy.
- **CNAME:** alias to another name.

Passive DNS may contain historic values. Do not treat an old record as proof the host is active. Do not attempt zone transfers or intrusive queries unless the room explicitly includes them and the target is authorised.

Useful lookups for an approved domain include:

```bash
dig TARGET_DOMAIN A
dig TARGET_DOMAIN MX
dig TARGET_DOMAIN NS
dig TARGET_DOMAIN TXT
```

Use the exact domain from the exercise. The response sections matter: a requested record may be absent even when the name exists.

## Phase 4: Certificates, Metadata, and Technology Clues

Certificate transparency logs can reveal hostnames that appeared in issued certificates. Public job listings, documentation, repositories, and document metadata can also expose technologies or naming conventions. These are leads, not proof that a service is currently running or vulnerable.

A finding such as “a certificate exists for api.example.com” establishes that the name appeared in a certificate record. It does not prove the host is still online, owned by the target, or insecure. Verify ownership and current relevance before making an active-test recommendation.

If a public file contains credentials or sensitive data, do not redistribute it. Record the minimum evidence necessary, report the exposure through an appropriate channel, and recommend revocation or rotation.

## Phase 5: Passive Versus Active Reconnaissance

Passive reconnaissance reads existing public information without directly probing the target's services. Active reconnaissance sends requests to the target, such as port scans, directory enumeration, or direct HTTP tests.

A public hostname may be a useful lead for later testing, but the next step must remain inside the authorised scope. Passive discovery is not a loophole for scanning unrelated infrastructure.

## Phase 6: Room Questions and Evidence Method

For each task, identify the type of information requested and use the source specified by the room. Record the exact field or result rather than inferring from a snippet.

| Task asks for | Source | What to capture |
|---|---|---|
| Domain registration detail | WHOIS or registrar record | Exact field name and value |
| DNS record | DNS query result | Record type and answer value |
| Certificate-related hostname | Certificate-transparency result | Hostname and certificate record |
| Public document | Search-engine result and original document | URL, title, date, relevant public field |
| Technology clue | Official documentation or observed page | Evidence and confidence, not just a guess |

If the live room expects a value from a particular source, use that source's current result. Do not copy stale values from an old walkthrough.

## Phase 7: Defensive Use and Key Takeaways

Defenders can use passive recon to inventory their external attack surface, identify abandoned hostnames, remove sensitive public documents, audit repository history, and monitor certificate issuance. Public exposure should be addressed at its source; deleting a current file does not erase copies or secrets from historical commits.

The key discipline is evidence quality. Record the source and time, confirm that the asset belongs to the target, distinguish observed facts from hypotheses, and do not move from passive research to active testing without explicit authorisation.
