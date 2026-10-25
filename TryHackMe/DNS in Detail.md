# DNS in Detail: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/dnsindetail

## Room Overview & Information

**Category:** Networking fundamentals  
**Difficulty:** Beginner  
**Focus:** DNS purpose, domain hierarchy, record types, recursive resolution, caching, and practical record lookups.

The Domain Name System translates human-readable domain names into records used by clients and services. It is more than a simple hostname-to-IP lookup: DNS also supports mail routing, aliases, service discovery, and domain ownership checks.

This is a fundamentals room with an interactive DNS query tool rather than a target to exploit. Do not invent scan output or claim a privilege-escalation stage that the room does not contain.

## Phase 1: Domain hierarchy

A domain is read from right to left. In `www.example.com`, `.com` is the top-level domain (TLD), `example` is the registered domain label, and `www` is a subdomain label. Organisations can create additional subdomains such as `api.dev.example.com`.

A fully qualified domain name identifies a complete name in the DNS hierarchy. DNS names are not the same thing as IP addresses; DNS records map names to data, and a single name may have multiple records or answers.

## Phase 2: Record types

| Record | Purpose | Example use |
|---|---|---|
| `A` | Maps a name to an IPv4 address | Website IPv4 address |
| `AAAA` | Maps a name to an IPv6 address | Website IPv6 address |
| `CNAME` | Alias to another DNS name | Store hostname pointing to a hosting provider |
| `MX` | Mail server and priority | Directs email delivery |
| `TXT` | Arbitrary text data | SPF policy, verification token, DMARC policy |
| `NS` | Authoritative nameservers | Delegation of a DNS zone |
| `SOA` | Zone authority and serial/timing data | Zone administration |
| `PTR` | Reverse lookup mapping | Address-to-name lookup in reverse DNS |

MX records include a preference number. Lower preference values are tried before higher values, assuming the server is available. A CNAME points to another name, not directly to an IP address, so another lookup may be needed.

## Phase 3: How a DNS query resolves

1. The client checks its local resolver cache.
2. If no usable cached answer exists, the client asks a recursive resolver.
3. The recursive resolver may answer from its own cache.
4. Otherwise, it follows referrals from root servers to the relevant TLD nameservers.
5. The authoritative nameserver returns the record for the requested name.
6. The resolver caches the answer according to its TTL and returns it to the client.

The TTL is measured in seconds and determines how long a resolver may cache an answer. A cached answer may remain visible until the TTL expires, so a recent DNS change is not necessarily reflected everywhere immediately.

## Phase 4: Query DNS from a terminal

The room's practical website provides a DNS query emulator and shows corresponding command examples. On a Linux system with DNS tools installed, the same ideas can be practised with `dig`.

Query an A record:

```bash
dig example.com A
```

Query a specific record type:

```bash
dig example.com MX
dig example.com TXT
dig example.com NS
```

Ask a particular resolver:

```bash
dig @1.1.1.1 example.com A
```

The `@` syntax selects the resolver. Replace example domains with the names authorised by your exercise. The `ANSWER SECTION` contains the records returned; `status: NOERROR` means the DNS response completed without a DNS-level error, not that the destination website is safe or reachable.

An alternative tool is:

```bash
nslookup -type=MX example.com
```

The exact output varies by resolver, cache, and domain configuration. Do not copy a sample IP address as though it were the answer for the room's test domain.

## Phase 5: Practical questions

The room's practical uses the interactive query tool for `website.thm`. Use the exact record name and type shown in each question. The expected values come from the room's simulated DNS zone, not from public DNS.

**Question: “What is the CNAME of shop.website.thm?”**

1. Open the DNS query emulator.
2. Enter `shop.website.thm` as the name.
3. Select the `CNAME` record type.
4. Run the query.
5. Read the alias from the answer section and enter it exactly as displayed.

A CNAME answer is a hostname, not an IPv4 address.

**Question: “What is the value of the TXT record of website.thm?”**

1. Query `website.thm`.
2. Select `TXT`.
3. Read the text value from the answer.
4. Preserve punctuation and quoting only as required by the answer field.

TXT data may contain a verification string or mail-security policy. Do not confuse the record's owner name with the record's text value.

**Question: “What is the numerical priority value for the MX record?”**

1. Query `website.thm`.
2. Select `MX`.
3. Read the numeric preference field before the mail server hostname.
4. Submit the number requested, not the hostname.

**Question: “What is the IP address for the A record of www.website.thm?”**

1. Query `www.website.thm`.
2. Select `A`.
3. Read the IPv4 address in the answer.
4. Submit the address from the lab response.

The exact values are generated by the active room's simulated DNS environment. This write-up explains how to retrieve them rather than substituting guessed answers from another instance.

## Phase 6: Common errors

- **NXDOMAIN:** the queried name does not exist in the DNS view used by the resolver.
- **NOERROR with no answer:** the name may exist but have no record of the requested type.
- **SERVFAIL:** the resolver failed to complete the lookup.
- **Timeout:** the resolver may be unreachable, the request may be filtered, or the service may not respond.
- **Unexpected old result:** a resolver may still have a cached answer within its TTL.

A DNS answer does not prove the service is reachable. After resolving the name, network routing, firewall policy, TLS, and the application can still fail independently.

## Phase 7: Security and mitigation

DNS information is public by design, but careless records can expose internal naming, stale hosts, or service metadata. Review public zones, remove obsolete records, protect registrar and DNS-provider accounts with MFA, and use DNSSEC where appropriate to provide authenticity of DNS data. DNSSEC does not encrypt DNS queries and does not make a malicious destination safe.

For email, configure SPF, DKIM, and DMARC carefully and monitor reports. Avoid placing secrets in TXT records because anyone who can query the zone can usually read them.

## Key takeaways

DNS is a distributed database with caching and delegation. Record types answer different questions, and the record value must be read from the correct query. For the practical, the correct method is to select the requested name and record type in the emulator, then copy the value returned by the lab exactly.

## Phase 8: Practical question checklist

| Question | Query | Field to submit |
|---|---|---|
| What is the CNAME of shop.website.thm? | Query shop.website.thm with type CNAME | Alias hostname |
| What is the TXT record value of website.thm? | Query website.thm with type TXT | Exact TXT text |
| What is the numerical priority value for the MX record? | Query website.thm with type MX | Numeric preference |
| What is the IP address for the A record of www.website.thm? | Query www.website.thm with type A | IPv4 address |

These values belong to the room's simulated DNS zone. The answer should come from the current emulator output; don't replace it with a plausible value from a public domain. Preserve the exact spelling and formatting.

## Final troubleshooting and defence

If a query fails, check the record type, spelling, resolver, cache/TTL, and response status before changing multiple things at once. NXDOMAIN, NOERROR-without-an-answer, SERVFAIL, and timeout indicate different failure classes.

For defensive practice, remove stale DNS records, protect registrar access with MFA, maintain SPF/DKIM/DMARC, monitor public changes, and avoid putting secrets into TXT records. DNSSEC can improve DNS data authenticity but does not encrypt queries or establish that a destination is trustworthy.
