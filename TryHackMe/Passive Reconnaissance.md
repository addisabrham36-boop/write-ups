# Passive Reconnaissance — TryHackMe

Room: https://tryhackme.com/room/passiverecon

Before testing a system, I need to understand what is publicly visible about it. That first information-gathering stage is reconnaissance. Passive reconnaissance means collecting information from public sources without directly probing the target's systems. It can still reveal a surprising amount, which is why organisations should pay attention to what they publish.

## What counts as passive reconnaissance?

Examples include reading a company's website, checking public DNS records, looking at certificate information, reviewing search-engine results, and searching public documents for details that were accidentally exposed. The key distinction is how the information is collected. I am observing information that is already available rather than actively sending scans or test traffic to the target.

Passive does not mean harmless in every context. Information can be sensitive, and I still need to respect privacy, laws, and the scope of any assessment.

## Checking DNS information

DNS helps translate names such as `example.com` into information that computers can use. On a Linux system, I can query public DNS records with tools such as `dig`:

```bash
dig example.com A
dig example.com MX
dig example.com NS
dig example.com TXT
```

These queries ask for different record types:

- `A` can return an IPv4 address.
- `MX` lists mail servers for the domain.
- `NS` lists authoritative name servers.
- `TXT` can contain verification data and other published text records.

The output depends on the domain's configuration and the resolver being used. A missing record does not automatically mean something is broken, and a result can change over time.

## WHOIS and public sources

A WHOIS lookup may show registration details for a domain or IP allocation, depending on the registry and privacy settings. For example:

```bash
whois example.com
```

Some registrant details may be hidden or replaced by a privacy service. I should not assume that every field is current or that the listed contact is the person who manages the technical system.

Other useful sources include search engines, public code repositories, certificate-transparency logs, job listings, and the organisation's own documentation. Each source gives only part of the picture, so I should cross-check important findings rather than treating one search result as unquestionable truth.

## Keeping the work organised

When I collect information, I should record the source and when I found it. That makes it easier to tell the difference between a verified fact and an assumption. I should also separate information about the organisation from information about third-party providers, because a domain or IP address can belong to a hosting company or service rather than directly to the target.

## What I took from this room

Passive reconnaissance is about building an initial picture from public information before moving to direct testing. DNS lookups, registration data, search engines, and public documents can reveal useful clues, but they need context. For an authorised assessment, this stage helps me decide what is worth investigating next while keeping the work within the agreed scope.

I have kept the commands here as general examples. I should only record specific discoveries or room answers after verifying them during the room itself.
