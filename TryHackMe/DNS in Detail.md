# DNS in Detail — TryHackMe

Room: https://tryhackme.com/room/dnsindetail

DNS is the system that helps turn domain names into information computers can use, such as IP addresses. Without it, I would have to remember addresses for every website and service. DNS is often described as the internet's phone book, although it can store more than just an address.

## Common record types

- **A:** maps a name to an IPv4 address.
- **AAAA:** maps a name to an IPv6 address.
- **CNAME:** points one name to another canonical name.
- **MX:** identifies mail servers for a domain.
- **NS:** identifies name servers authoritative for a zone.
- **TXT:** stores text data, often used for verification and email-security policies.
- **PTR:** supports reverse lookups from an address to a name.

I can inspect records from Linux using `dig`:

```bash
dig example.com A
dig example.com AAAA
dig example.com MX
dig example.com NS
dig example.com TXT
```

The domain here is an example, not a lab target.

## How a lookup works

A device usually asks a recursive resolver for a result. If the resolver does not already have a cached answer, it can follow the DNS hierarchy, starting with root servers, then the relevant top-level domain servers, and finally the domain's authoritative name servers. The resolver returns the answer to the client and may cache it for the record's TTL.

Caching makes lookups faster, but it also means that DNS changes may not appear everywhere immediately. The TTL helps determine how long a record can be cached.

## Security relevance

Incorrect DNS records can disrupt services or expose information. DNS is also used in phishing, command-and-control traffic, and other attacks, so defenders often inspect unusual query patterns. DNSSEC can help validate the authenticity and integrity of DNS data, but it does not encrypt ordinary DNS queries.

## What I took from this room

DNS is hierarchical, cached, and built around different record types. When troubleshooting or investigating a domain, I should ask which record I need and what resolver answered. A DNS result is useful evidence, but it needs context and may change over time.