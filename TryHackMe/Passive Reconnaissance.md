# Passive Reconnaissance: TryHackMe Walkthrough

Official room: https://tryhackme.com/room/passiverecon

## Room Overview & Information

**Category:** OSINT and reconnaissance  
**Difficulty:** Beginner  
**Focus:** Passive versus active reconnaissance, WHOIS, DNS lookups, DNSDumpster, and Shodan.

Passive reconnaissance collects information from existing public sources without directly probing the target's services. Active reconnaissance interacts with the target, for example by pinging a host or scanning its ports. This room uses public information and a simulated DNS lab to teach the difference.

## Task 2: Passive Versus Active Recon

### Question: “You visit the Facebook page of the target company, hoping to get some of their employee names. What kind of reconnaissance activity is this?”

**Answer: P (Passive).**

The researcher is looking at information already published on a third-party social platform and is not directly probing the target's systems.

### Question: “You ping the IP address of the company webserver to check if ICMP traffic is blocked. What kind of reconnaissance activity is this?”

**Answer: A (Active).**

Ping sends network traffic to the target and observes the response. It can appear in logs and is therefore active reconnaissance.

### Question: “You happen to meet the IT administrator of the target company at a party. You try to use social engineering to get more information about their systems and network infrastructure. What kind of reconnaissance activity is this?”

**Answer: A (Active).**

This is direct interaction with a person to obtain information. The room classifies this as active reconnaissance because the researcher is directly engaging the target organisation. In a real engagement, social engineering must be explicitly included in the written scope.

## Task 3: WHOIS

WHOIS returns domain-registration details such as registrar, registration dates, and nameservers. Some fields may be privacy-protected, and results vary by registry and date.

Run:

```bash
whois tryhackme.com
```

Read the relevant field rather than relying on a search snippet.

### Question: “When was TryHackMe.com registered?”

**Room answer:** `20180705`.

This is the date format used by the room's WHOIS result: YYYYMMDD.

### Question: “What is the registrar of TryHackMe.com?”

**Room answer:** `namecheap.com`.

### Question: “Which company is TryHackMe.com using for name servers?”

**Room answer:** `CLOUDFLARE.COM`.

WHOIS data can change. If the current room or live lookup returns a different value, follow the active task's data and record the query date.

## Task 4: nslookup and dig

DNS maps names to records. Different record types answer different questions:
- **A:** IPv4 address.
- **AAAA:** IPv6 address.
- **MX:** mail exchanger and preference.
- **NS:** authoritative nameserver.
- **TXT:** text data, often used for verification or email policy.

The room asks you to query the TXT record for thmlabs.com. Use either tool:

```bash
nslookup -type=TXT thmlabs.com
dig thmlabs.com TXT
```

- **nslookup -type=TXT** asks for TXT records.
- **dig thmlabs.com TXT** performs the same query using dig.
- The answer section contains the record value.

### Question: “What is the TXT record value for thmlabs.com?”

The record contains a flag. Obtain it from the active room's DNS result and submit it there. The flag is deliberately redacted in this public write-up as `THM{hidden}`.

A DNS record is public data, not encrypted secret storage. Do not place credentials or private keys in TXT records.

## Task 5: DNSDumpster

DNSDumpster aggregates publicly available DNS and related information. Search for `tryhackme.com` and inspect the subdomains listed in the result. The question asks for an interesting subdomain in addition to `www` and `blog`.

### Question: “Lookup tryhackme.com on DNSDumpster. What is one interesting subdomain that you would discover in addition to www and blog?”

**Room answer:** `remote`.

DNSDumpster results can change over time. Use the active room's expected result if the current site presents different data, and do not assume that a discovered hostname is currently online or vulnerable.

## Task 6: Shodan.io

Shodan indexes publicly reachable services observed by its crawlers. Search results include service banners, ports, and other metadata. They represent observations at a point in time and should not be treated as proof that a service is still available.

### Question: “According to Shodan.io, what is the 2nd country in the world in terms of the number of publicly accessible Apache servers?”

**Room answer:** **Germany**.

### Question: “Based on Shodan.io, what is the 3rd most common port used for Apache?”

**Room answer:** **8080**.

### Question: “Based on Shodan.io, what is the 3rd most common port used for nginx?”

**Room answer:** **8888**.

Shodan's rankings can change as it scans new systems and changes its classification. These are the room's expected answers, not permanent global facts. If the room's live dashboard differs, follow its current result.

## Reporting and responsible use

For every discovery, record the source, query, date, observation, and confidence. A certificate record proves a hostname appeared in a certificate; it does not prove the host is currently online. A Shodan result shows an observed service, not necessarily a current vulnerability.

Do not attempt to access or exploit discovered systems unless they are within the authorised scope. Publicly indexed information can still be sensitive, so minimise collection and report accidental exposure responsibly.

## Key takeaways

Passive reconnaissance uses public information; active reconnaissance interacts directly with a target. WHOIS provides registration context, DNS queries return record data, DNSDumpster helps discover subdomains, and Shodan summarises observed internet-facing services. Verify results, record when they were obtained, and avoid turning a clue into an unsupported conclusion.

## Phase 5: Verification, Reporting, and Defence

### Verify findings before reporting

Use each source for the type of fact it can actually establish. WHOIS supports conclusions about registration metadata at the time of the query. DNS supports conclusions about returned records. DNSDumpster can suggest candidate subdomains. Shodan reports observations of services indexed by its crawlers. None of these alone proves that a host is currently online, vulnerable, or controlled by the organisation.

For each result, save the source URL or command, the collection date, the exact returned value, and a confidence note. If sources disagree, document the discrepancy rather than selecting the value that looks most plausible.

### Command reference

    whois TARGET_DOMAIN
    dig TARGET_DOMAIN A
    dig TARGET_DOMAIN MX
    dig TARGET_DOMAIN NS
    dig TARGET_DOMAIN TXT
    nslookup -type=TXT TARGET_DOMAIN

Replace TARGET_DOMAIN with the domain asked about in the room. WHOIS queries registration metadata; dig asks for a specific DNS record type; nslookup performs a similar DNS lookup. The query type matters: an MX record answers mail-routing questions, while an A record answers an IPv4 mapping question.

### Room-answer checklist

| Topic | What to check |
|---|---|
| Passive vs active reconnaissance | Whether the action reads existing public information or sends traffic to the target |
| Registration date | WHOIS creation/registered-on field and date format |
| Registrar | Registrar field, not the registrant or nameserver provider |
| Nameservers | NS/WHOIS nameserver entries |
| TXT flag | Entire TXT value returned by the lab's DNS query |
| Interesting subdomain | Exact DNSDumpster result expected by the room |
| Shodan rankings | The room's current displayed country or port ranking |

The Shodan rankings are time-sensitive. The answers included above are the documented room answers, but if the active task shows updated rankings, use the currently displayed result. Keep all active testing within explicit scope and do not probe a discovered host merely because it appeared in a public data source.
