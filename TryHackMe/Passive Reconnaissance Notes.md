# Passive Reconnaissance: Practical Notes

Room: https://tryhackme.com/room/passiverecon

Passive reconnaissance gathers information from existing public sources without directly probing the target's services. It is useful early in an assessment because it can identify domains, public assets, technologies, and potential exposure while keeping direct interaction with the target to a minimum.

## 1. Define the scope

Record the organisation, domain names, and any restrictions given by the engagement or room. A similar domain name may belong to another organisation, and public infrastructure may be shared by many customers. Confirm that each finding belongs to the intended target before reporting it.

## 2. Search public information

Useful sources include search engines, official websites, public documentation, certificate transparency logs, public DNS records, and approved internet search services. Search operators can narrow results to a domain, file type, title, or URL pattern.

Examples:
- site:example.com
- site:example.com filetype:pdf
- site:example.com intitle:login
- Search for a known domain in public certificate records.

Replace example.com with the authorised domain. Public search results can be stale or incomplete, so record the source and date.

## 3. DNS and domain information

DNS records can reveal IP addresses, mail providers, name servers, and subdomains. A records map names to IPv4 addresses, AAAA records to IPv6, MX records identify mail exchangers, NS records identify authoritative name servers, and TXT records can publish verification or email-security policy.

Passive DNS data may reflect historical values. Confirm whether an address is current before treating it as part of the active attack surface. Do not attempt zone transfers or intrusive queries unless the scope explicitly allows them.

## 4. Certificate and technology clues

Public certificate records can reveal hostnames that have used a domain. HTTP headers, public job listings, documentation, and error pages may reveal technologies or vendors. These clues can help plan later authorised testing, but they do not prove a vulnerability.

Avoid over-relying on a single technology fingerprint. Proxies can change headers, and organisations may use multiple stacks across different services.

## 5. Document findings

For each discovery, record the source, URL or query, date, observation, and confidence. Separate the fact from the inference. For example, a certificate record is evidence that a hostname appeared in a certificate; it is not proof that the host is currently online.

If public documents appear to expose sensitive information, do not redistribute the contents. Report the exposure through the appropriate channel and minimise collection.

## 6. Passive versus active reconnaissance

Passive work relies on existing public information. Active reconnaissance interacts with the target, for example by scanning ports or sending application requests. A search result may lead to an active test, but that next step requires explicit scope and authorisation.

## What I took from this room

Passive reconnaissance builds an initial picture from public evidence. Its value comes from accurate attribution, source tracking, and careful separation between what is known and what still needs to be verified.

## Expanded research workflow

### 1. Keep passive and active techniques separate

Passive recon uses public sources or information already available without directly probing the target. Active recon sends traffic to the target. A public hostname discovered through a search engine is not automatically in scope for active testing.

### 2. Record source and confidence

For each finding, record the value, source URL, collection time, and confidence. Public data can be stale, duplicated, or misattributed. A subdomain in an old document does not prove that the host is currently operated by the target.

### 3. Use common public sources

Relevant sources can include official web pages, registrar and DNS records, certificate-transparency logs, public documentation, public repositories, search engines, and metadata in documents that are intentionally public. Avoid collecting private personal information unless it is necessary and explicitly within the exercise.

### 4. Verify before acting

Corroborate important findings with a second source when possible. Keep facts separate from hypotheses, and never treat a search snippet as definitive when the source itself can be inspected. If a secret is found in a public source, do not reproduce it; recommend removal and rotation.

### 5. Turn recon into defensive work

Defenders can use the same process to inventory their external attack surface, remove abandoned DNS records, reduce unnecessary public metadata, and monitor newly issued certificates. Passive recon is most useful when findings lead to verified asset ownership and a controlled next step.

### 6. Room questions

For each task, use the specific public source identified in the room and record the exact value it shows. Preserve the original spelling, domain structure, and relevant date. Do not guess when a record is unavailable; note that it could not be verified in the current environment.
