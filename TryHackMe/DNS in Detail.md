# DNS in Detail: TryHackMe Walkthrough

Room: https://tryhackme.com/room/dnsindetail

The Domain Name System (DNS) maps names such as `example.com` to records that clients and services can use. It is often described as the internet's phone book, but it stores several kinds of records and supports more than simple hostname-to-IP lookups.

## 1. How a lookup works

When an application needs an address for a hostname, it usually asks a configured resolver. The resolver may already have a cached answer. Otherwise, it can follow the DNS hierarchy: root servers direct it to the appropriate top-level domain servers, which direct it toward the authoritative servers for the domain.

The authoritative server provides records for the domain. Results can be cached according to their TTL (time to live), so different clients may temporarily see different answers while changes propagate through caches.

A recursive resolver performs the lookup work for the client. An authoritative server provides definitive records for the zones it serves. These are different roles even though a provider may operate both kinds of service.

## 2. Common record types

| Record | Purpose |
|---|---|
| A | Maps a name to an IPv4 address |
| AAAA | Maps a name to an IPv6 address |
| CNAME | Alias from one name to another canonical name |
| MX | Mail exchanger for a domain |
| NS | Name servers authoritative for a zone |
| TXT | Text data, often used for verification and email security policies |
| PTR | Reverse lookup from an address to a name |
| SOA | Zone authority and timing information |

A CNAME points to another name, not directly to a numeric IP address. MX records identify mail servers and may have priorities. TXT records can contain important security configuration, but their presence alone does not prove a domain is secure.

## 3. Query DNS from Linux

The `dig` command is useful when installed:

```bash
dig example.com A
dig example.com AAAA
dig example.com MX
dig example.com NS
dig example.com TXT
```

Each command asks for a particular record type. Replace `example.com` with the domain provided by the room or one you are authorised to investigate.

For a concise answer, use:

```bash
dig +short example.com A
```

To query a particular resolver, specify it with `@SERVER_IP`. This can help compare resolver behaviour, but the server must be one you are authorised to use.

## 4. DNS and email security

Email systems commonly use DNS to publish MX records and policies. SPF uses TXT records to describe which senders are authorised to send mail for a domain. DKIM publishes a public key that recipients can use to verify signatures. DMARC tells receiving systems how to handle messages that fail alignment checks and where reports may be sent.

These mechanisms help with sender validation, but they do not make every message trustworthy. A domain can be configured correctly while an attacker uses a lookalike domain or a compromised legitimate account.

## 5. DNS security and reconnaissance

DNS can reveal subdomains, mail providers, service infrastructure, and configuration details. Zone transfers should be restricted to authorised secondary servers. Public records are intended to be queried, but sensitive internal names should not be exposed unnecessarily.

DNS tunnelling can encode data inside queries and responses. Defenders can look for unusual query volume, long or high-entropy subdomain labels, unexpected record types, and communication patterns that do not fit normal resolver use.

DNSSEC adds cryptographic validation of DNS data. It helps detect forged or modified answers, but it does not encrypt ordinary DNS queries and does not make a malicious domain safe.

## 6. Troubleshooting a lookup

If a hostname does not resolve:

1. Confirm the spelling and domain.
2. Run `dig hostname A` and inspect the status and answer sections.
3. Check whether the resolver is reachable.
4. Compare the response from an approved alternative resolver if the lab permits it.
5. Check local configuration and caches.
6. Distinguish a DNS failure from a connection failure to an address that already resolved.

A DNS response with no answer is not necessarily the same as a nonexistent domain. The status code and record type matter. NXDOMAIN indicates that the queried name does not exist according to the responding DNS server; a NOERROR response with no requested record can mean the name exists but has no record of that type.

## 7. Working through the room

For each question, identify which DNS role or record type is being described. If the task provides a domain, query the requested record type and use the actual answer. Do not substitute an IP address from an unrelated example. If the room asks about caching, TTL, recursion, or authority, explain the role each plays in the lookup process.

## What I took from this room

DNS is both a core dependency and a useful source of security information. Knowing the record types, resolver hierarchy, caching behaviour, and failure modes makes it easier to troubleshoot applications and recognise suspicious DNS activity.