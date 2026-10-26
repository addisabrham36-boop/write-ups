# DNS in Detail

{% embed url="https://tryhackme.com/room/dnsindetail" %}

A foundational networking room explaining the Domain Name System (DNS), domain hierarchy, record types, request resolution flow, and querying records using an interactive DNS simulator.

---

## 1. What is DNS?

DNS stands for **Domain Name System**. It acts as the internet's phonebook, translating human-friendly hostnames (like `tryhackme.com`) into computer-routable IP addresses (like `104.26.11.229`).

---

## 2. Domain Hierarchy

Domain names follow a tree structure processed from right to left:

```
Root (.) -> TLD (.com, .org, ccTLD) -> Second-Level Domain (tryhackme) -> Subdomain (blog)
```

- **ccTLD:** Country Code Top-Level Domain (e.g., `.uk`, `.de`, `.co.uk`).
- **gTLD:** Generic Top-Level Domain (e.g., `.com`, `.org`, `.net`).
- **Maximum length of a subdomain:** 63 characters.
- **Maximum length of a full domain name:** 253 characters.
- **Allowed characters:** Letters (`a-z`), digits (`0-9`), and hyphens (`-`). Underscores (`_`) cannot be used in subdomains/hostnames.

---

## 3. DNS Record Types

Common DNS records:
- **A:** Maps a hostname to an IPv4 address.
- **AAAA:** Maps a hostname to an IPv6 address.
- **CNAME:** Canonical Name (alias) pointing one domain to another domain.
- **MX:** Mail Exchange record specifying the mail server responsible for accepting emails for the domain (includes numeric priority).
- **TXT:** Free-form text fields, commonly used for SPF, DKIM, and domain verification tokens.

---

## 4. Making a Request

When a client queries a domain:
1. **Local Cache / Hosts File:** Checks local system memory.
2. **Recursive DNS Resolver:** Typically provided by your ISP or a public resolver (like `1.1.1.1` or `8.8.8.8`).
3. **Root Nameservers (`.`):** Directs the resolver to the appropriate TLD nameserver.
4. **TLD Nameservers:** Directs the resolver to the authoritative nameservers for the specific domain.
5. **Authoritative Nameserver:** Holds the official, authoritative records for the domain.
6. **TTL (Time to Live):** Specifies how long resolvers and clients should cache the response before querying again.

---

## 5. Practical Simulator Walkthrough

Using the interactive DNS simulator provided in the room:

1. Query `shop.website.thm` with type **CNAME**:
   Returns `shops.myshopify.com`.
2. Query `website.thm` with type **TXT**:
   Returns the room flag `flag{hidden}`.
3. Query `website.thm` with type **MX**:
   Returns mail server with priority `30`.
4. Query `www.website.thm` with type **A**:
   Returns `10.10.10.10`.

---

## 🏁 Questions & Answers

| Task | Question | Answer |
|---|---|---|
| **Task 1** | What does DNS stand for? | `Domain Name System` |
| **Task 2** | What is the maximum length of a subdomain? | `63` |
| **Task 2** | Which character cannot be used in a subdomain (`3`, `b`, `_`, `-`)? | `_` |
| **Task 2** | What is the maximum length of a domain name? | `253` |
| **Task 2** | What type of TLD is `.co.uk`? | `ccTLD` |
| **Task 3** | What type of record advises where to send email? | `MX` |
| **Task 3** | What type of record handles IPv6 addresses? | `AAAA` |
| **Task 4** | What field specifies how long a DNS record should be cached? | `TTL` |
| **Task 4** | What type of DNS Server is usually provided by your ISP? | `recursive` |
| **Task 4** | What type of server holds all the records for a domain? | `authoritative` |
| **Task 5** | What is the CNAME of `shop.website.thm`? | `shops.myshopify.com` |
| **Task 5** | What is the value of the TXT record of `website.thm`? | `flag{hidden}` |
| **Task 5** | What is the numerical priority value for the MX record? | `30` |
| **Task 5** | What is the IP address for the A record of `www.website.thm`? | `10.10.10.10` |
