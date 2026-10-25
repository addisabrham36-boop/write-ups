# Passive Reconnaissance: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/passiverecon

## Room Overview & Information

**Category:** Network security and OSINT  
**Difficulty:** Beginner  
**Focus:** WHOIS, DNS records, DNSDumpster, Certificate Transparency, and Shodan.

Passive reconnaissance collects information from public sources without directly probing the target's services. It can reveal domain registration information, DNS configuration, subdomains, certificates, and internet-exposed services. Public does not mean harmless: only investigate domains and assets within the exercise's scope, and handle discovered personal or sensitive information responsibly.

The official room notes that some public lookups require internet access. If the AttackBox cannot reach an external site, use the supported TryHackMe VPN environment or the room's instructions rather than inventing results.

## Phase 1: WHOIS

WHOIS can return domain-registration data such as registrar, creation or expiry dates, nameservers, and registrant details where those details are not privacy-protected.

Run a lookup on a domain that the room authorises:

```bash
whois example.com
```

The command queries a WHOIS service and prints the available registration record. The exact fields depend on the registry, registrar, and privacy settings. Do not assume that a missing registrant name means the domain has no owner; privacy services and registry policies often hide personal information.

Look for the registrar, important dates, and nameservers. Treat WHOIS as one source of context, not proof that a site is trustworthy.

## Phase 2: DNS records

DNS records expose how names map to services. Query the record types relevant to the domain:

```bash
dig example.com A
dig example.com AAAA
dig example.com MX
dig example.com TXT
dig example.com NS
```

- `A` returns IPv4 addresses.
- `AAAA` returns IPv6 addresses.
- `MX` identifies mail exchangers and their preference values.
- `TXT` returns text records, which may include email policy or verification data.
- `NS` lists authoritative nameservers.

For a name you already know, these commands answer a specific question. They do not enumerate every possible subdomain.

## Phase 3: Discover subdomains with DNSDumpster

DNSDumpster aggregates publicly available DNS and related information. In the room's exercise, search for the domain specified by the task and review the results. Record the discovered subdomain, resolved IP, and record type as shown.

The purpose is to discover names that are not obvious from the homepage. A forgotten development host or old admin portal may enlarge an organisation's attack surface. Do not assume a discovered subdomain is vulnerable; passive discovery only identifies a possible asset.

If the service does not load, check the room's connectivity guidance. Do not substitute a guessed hostname or a result from another domain.

## Phase 4: Certificate Transparency

Certificate Transparency (CT) logs are public append-only records of issued TLS certificates. Certificates often contain domain names and subdomains in their Subject Alternative Name list. Search the domain through a CT search service such as crt.sh and review matching certificate names.

Example command-line query for a public CT site, where network access is available:

```bash
curl -s 'https://crt.sh/?q=%25.example.com&output=json'
```

This is an illustrative query pattern. It asks crt.sh for certificate entries matching subdomains of example.com and requests JSON output. The returned data can include duplicates, expired certificates, and historical names, so validate any candidate before treating it as a current asset.

A certificate entry shows that a name appeared in a certificate record; it does not prove that the hostname currently resolves or serves a live application.

## Phase 5: Shodan

Shodan indexes information about internet-facing services observed by its crawlers. Search the domain or a resolved address using the room's authorised example. Review the service, port, banner, and timestamp when available.

A Shodan result is a historical observation, not a guarantee that the service is still online. Service banners can be inaccurate or intentionally altered. Use the results to form questions for an authorised assessment, not to claim a confirmed vulnerability.

## Phase 6: Answering the room's practical questions

The room uses public lookups and example domains. For every question asking for a specific domain, subdomain, IP, or service:

1. Copy the exact domain from the question.
2. Use the specified public source (WHOIS, DNS, DNSDumpster, CT logs, or Shodan).
3. Identify the field the question asks for.
4. Record the exact result and its source.
5. If the result is historical or duplicated, verify it before drawing conclusions.

The task values are dependent on the queried domain and the data available when the lookup is performed. This guide describes the procedure rather than fabricating a supposedly universal answer.

## Phase 7: Passive versus active reconnaissance

A DNS query to a public resolver, WHOIS lookup, CT search, or Shodan query generally relies on third-party/public data and is considered passive relative to the target. A direct port scan, HTTP request to the target, or login attempt is active and can be logged by the target.

The distinction is about how information is gathered, not whether the activity is automatically authorised. Keep both passive and active work inside the approved scope.

## Phase 8: Reporting and mitigation

Maintain an asset inventory, remove obsolete DNS records, protect registrar accounts with MFA, and review public service exposure. Avoid publishing discovered personal information or sensitive host details unnecessarily. Passive data can be incomplete or stale, so label the source and observation time in a report.

## Key takeaways

WHOIS gives registration context, DNS reveals configured records, DNSDumpster helps surface subdomains, CT logs reveal names in certificates, and Shodan shows observed internet-facing services. Cross-check sources and treat every result as a lead that requires validation.