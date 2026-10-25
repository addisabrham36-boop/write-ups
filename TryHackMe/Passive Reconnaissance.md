# Passive Reconnaissance: Complete TryHackMe Walkthrough

Official room: https://tryhackme.com/room/passiverecon

## Room Overview & Information

**Category:** Open-source intelligence and reconnaissance  
**Difficulty:** Beginner  
**Topics:** Passive versus active reconnaissance, WHOIS, DNS lookups, DNSDumpster, and Shodan.

Passive reconnaissance collects information from public sources without directly probing the target's services. Active reconnaissance sends traffic to the target, such as pinging a host or scanning ports. This room combines knowledge checks with public-information lookups.

Public records change over time. Answers below are the documented room answers; if the live task explicitly displays a newer value, use the current task result and record when you observed it.

## Phase 1: Reconnaissance & Enumeration Concepts

### Task 1: Introduction

Reconnaissance collects information that helps an analyst or tester understand the target before deeper assessment. It may identify domains, subdomains, technologies, people, and publicly observed services. Public discovery is not permission to access or exploit a discovered host.

### Task 2: Passive Versus Active Recon

**Question:** “You visit the Facebook page of the target company, hoping to get some of their employee names. What kind of reconnaissance activity is this?”

**Answer:** **P**, for Passive.

You are reading information already published on a third-party platform rather than sending a probe to the target's infrastructure.

**Question:** “You ping the IP address of the company webserver to check if ICMP traffic is blocked. What kind of reconnaissance activity is this?”

**Answer:** **A**, for Active.

Ping sends network traffic to the target and observes the response.

**Question:** “You happen to meet the IT administrator of the target company at a party. You try to use social engineering to get more information about their systems and network infrastructure. What kind of reconnaissance activity is this?”

**Answer:** **A**, for Active, according to the room.

The interaction directly engages a person to obtain information. In a real engagement, social engineering must be explicitly included in the written scope.

## Phase 2: Public-Record and DNS Enumeration

### Task 3: WHOIS

WHOIS returns domain-registration information such as registrar, registration dates, and nameserver data. Privacy services may hide registrant details.

Run:

~~~bash
whois tryhackme.com
~~~

Read the relevant field in the output instead of trusting a search snippet.

**Question:** “When was TryHackMe.com registered?”

**Documented room answer:** **20180705**.

The room uses YYYYMMDD format.

**Question:** “What is the registrar of TryHackMe.com?”

**Documented room answer:** **namecheap.com**.

**Question:** “Which company is TryHackMe.com using for name servers?”

**Documented room answer:** **CLOUDFLARE.COM**.

The registrar and nameserver operator are different roles. Do not substitute the DNS provider for the registrar.

### Task 4: nslookup and dig

DNS maps names to records. Common record types include:
- **A:** IPv4 address.
- **AAAA:** IPv6 address.
- **MX:** mail exchanger and preference.
- **NS:** authoritative nameserver.
- **TXT:** text value often used for verification or email policy.
- **PTR:** reverse mapping from an IP address to a hostname.

The room asks for the TXT record of thmlabs.com. Query it with either command:

~~~bash
nslookup -type=TXT thmlabs.com
dig thmlabs.com TXT
~~~

The first command requests TXT records through nslookup. The second performs the same query with dig. Copy the TXT value from the answer section exactly.

**Question:** “What is the TXT record value for thmlabs.com?”

**Answer:** Retrieve the complete value returned by the live DNS query and submit it in the room. The value is redacted here as **THM{hidden}** because it was not independently verified in this session.

A DNS record is public data, not encrypted secret storage. Do not place credentials or private keys in TXT records.

### Task 5: DNSDumpster

DNSDumpster aggregates publicly available DNS and related infrastructure data. Search for **tryhackme.com**, then inspect the subdomains shown in the results.

**Question:** “Lookup tryhackme.com on DNSDumpster. What is one interesting subdomain that you would discover in addition to www and blog?”

**Documented room answer:** **remote**.

DNSDumpster results can change. A subdomain in a public data source does not prove that the host is currently online or vulnerable.

### Task 6: Shodan.io

Shodan indexes publicly reachable services observed by its crawlers. Results can show service banners, ports, and country rankings. These are observations from a point in time.

**Question:** “According to Shodan.io, what is the 2nd country in the world in terms of the number of publicly accessible Apache servers?”

**Documented room answer:** **Germany**.

**Question:** “Based on Shodan.io, what is the 3rd most common port used for Apache?”

**Documented room answer:** **8080**.

**Question:** “Based on Shodan.io, what is the 3rd most common port used for nginx?”

**Documented room answer:** **8888**.

These are the room's documented ranking values, not permanent global facts. If the current room displays different rankings, use its current result.

## Phase 3: Verify, Correlate, and Report Findings

For every discovery, record the source, query, collection date, exact observation, and confidence. WHOIS supports conclusions about registration data at the time of the query. DNS supports conclusions about returned records. DNSDumpster can suggest candidate subdomains. Shodan reports services seen by its crawlers. None alone proves that a host is currently online, vulnerable, or controlled by the organisation.

Do not attempt to log in to or exploit discovered systems unless they are explicitly within authorised scope. Publicly indexed data can still be sensitive; minimise collection and report accidental exposure responsibly.

## Phase 4: Room Questions & Answers

| Task | Question focus | Answer |
|---|---|---|
| 2 | Facebook page of target company: passive or active? | P |
| 2 | Ping target's webserver IP: passive or active? | A |
| 2 | Social-engineer the IT administrator: passive or active? | A |
| 3 | TryHackMe.com registration date | 20180705 |
| 3 | Registrar | namecheap.com |
| 3 | Nameserver company | CLOUDFLARE.COM |
| 4 | TXT record value for thmlabs.com | Read exact record from live query; redacted as THM{hidden} |
| 5 | Interesting subdomain beyond www and blog | remote |
| 6 | Second country for public Apache servers | Germany |
| 6 | Third most common Apache port | 8080 |
| 6 | Third most common nginx port | 8888 |

## Phase 5: Key Takeaways & Remediation

Passive reconnaissance uses existing public sources; active reconnaissance interacts directly with the target. WHOIS provides registration context, DNS returns records, DNSDumpster helps identify candidate subdomains, and Shodan summarises observed internet-facing services.

For defenders, maintain an inventory of public assets, remove abandoned DNS records, avoid exposing sensitive data in public documents and repositories, monitor newly issued certificates, and restrict unnecessary services. Keep evidence reproducible and do not turn a public clue into an unsupported claim about current vulnerability.